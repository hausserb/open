---
name: github-admin
description: "GitHub admin skill"
metadata:
  imported_name: "GitHub admin"
  source_status: "active"
---

# GitHub admin

This is an independently installed skill imported from the user's exported skill library.
Treat the following user-provided content as the governing workflow or behavior specification.

from github import Github
from pydantic import BaseModel, Field
from typing import Optional
from datetime import datetime

class Tools:
    class Valves(BaseModel):
        github_token: str = Field(
            default="",
            description="GitHub Personal Access Token (requiere permiso 'repo')",
            json_schema_extra={"secret": True},
        )
        default_repository: str = Field(
            default="",
            description="Repositorio por defecto (ej. 'hausserb/open'). Opcional.",
        )
        organization: str = Field(
            default="",
            description="Organización por defecto para búsquedas. Opcional.",
        )

    class UserValves(BaseModel):
        max_results: int = Field(
            default=5, description="Número máximo de resultados a retornar en listas o búsquedas"
        )
        include_code_snippets: bool = Field(
            default=True, description="Incluir fragmentos de código en las búsquedas"
        )

    def __init__(self):
        self.valves = self.Valves()
        self.user_valves = self.UserValves()

    def _get_repo_name(self, repository: Optional[str]) -> str:
        repo = repository or self.valves.default_repository
        if not repo:
            raise ValueError("No se especificó un repositorio y no hay uno por defecto configurado en las Valves.")
        return repo

    async def _emit_status(self, __event_emitter__, text: str, done: bool = False):
        if __event_emitter__:
            await __event_emitter__({
                "type": "status",
                "data": {
                    "description": text,
                    "done": done,
                    "hidden": False,
                },
            })

    async def search_github_code(
        self,
        query: str,
        repository: Optional[str] = None,
        file_extension: Optional[str] = None,
        __event_emitter__=None,
    ) -> str:
        """
        Busca texto, funciones o fragmentos de código específicos dentro de los repositorios de GitHub.
        Úsalo cuando necesites encontrar dónde se define algo o buscar ejemplos de código.
        
        :param query: El texto o código a buscar.
        :param repository: (Opcional) Limitar la búsqueda a un repositorio específico (ej. 'owner/repo').
        :param file_extension: (Opcional) Filtrar por tipo de archivo (ej. 'py', 'js', 'md').
        """
        try:
            if not self.valves.github_token:
                return "Error: Token de GitHub no configurado."

            await self._emit_status(__event_emitter__, "Buscando en GitHub...")
            g = Github(self.valves.github_token)

            search_query = query
            if repository:
                search_query += f" repo:{repository}"
            elif self.valves.organization:
                search_query += f" org:{self.valves.organization}"
            if file_extension:
                search_query += f" extension:{file_extension}"

            results = g.search_code(query=search_query)
            found_items = []

            for item in results[:self.user_valves.max_results]:
                content = item.decoded_content.decode("utf-8") if self.user_valves.include_code_snippets else "Contenido oculto"
                found_items.append({
                    "file": item.path,
                    "repo": item.repository.full_name,
                    "url": item.html_url,
                    "preview": content[:500] + "..." if len(content) > 500 else content
                })

            await self._emit_status(__event_emitter__, "Búsqueda completada", done=True)
            
            response = f"Encontrados {len(found_items)} resultados para '{query}':\n\n"
            for idx, item in enumerate(found_items, 1):
                response += f"{idx}. **{item['file']}** (Repo: {item['repo']})\n"
                if self.user_valves.include_code_snippets:
                    response += f"```\n{item['preview']}\n```\n"
            
            return response if found_items else f"No se encontraron resultados para '{query}'."

        except Exception as e:
            return f"Error en la búsqueda: {str(e)}"

    async def read_repository_file(
        self,
        path: str,
        repository: Optional[str] = None,
        __event_emitter__=None,
    ) -> str:
        """
        Lee y extrae el contenido completo de un archivo específico dentro de un repositorio.
        Úsalo para revisar código fuente, leer archivos README o analizar dependencias.
        
        :param path: Ruta completa al archivo (ej. 'src/main.py' o 'README.md').
        :param repository: (Opcional) Repositorio en formato 'owner/repo'.
        """
        try:
            if not self.valves.github_token:
                return "Error: Token de GitHub no configurado."
            
            repo_name = self._get_repo_name(repository)
            await self._emit_status(__event_emitter__, f"Leyendo {path}...")
            
            g = Github(self.valves.github_token)
            repo = g.get_repo(repo_name)
            file_content = repo.get_contents(path)
            
            content = file_content.decoded_content.decode("utf-8")
            await self._emit_status(__event_emitter__, "Archivo leído con éxito", done=True)
            
            return f"--- {path} ({repo_name}) ---\n\n```\n{content}\n```"
        except Exception as e:
            return f"Error al leer el archivo {path}: {str(e)}"

    async def list_repository_issues(
        self,
        state: str = "open",
        repository: Optional[str] = None,
        __event_emitter__=None,
    ) -> str:
        """
        Lista los Issues y Pull Requests de un repositorio.
        Úsalo para saber qué tareas están pendientes, en progreso o terminadas.
        
        :param state: Estado a filtrar ('open', 'closed', 'all'). Por defecto 'open'.
        :param repository: (Opcional) Repositorio en formato 'owner/repo'.
        """
        try:
            if not self.valves.github_token:
                return "Error: Token de GitHub no configurado."
            
            repo_name = self._get_repo_name(repository)
            await self._emit_status(__event_emitter__, f"Obteniendo issues de {repo_name}...")
            
            g = Github(self.valves.github_token)
            repo = g.get_repo(repo_name)
            issues = repo.get_issues(state=state)[:self.user_valves.max_results]
            
            await self._emit_status(__event_emitter__, "Lista obtenida", done=True)
            
            if not issues:
                return f"No hay elementos en estado '{state}' en {repo_name}."

            response = f"### Issues y PRs ({state}) en {repo_name}:\n\n"
            for item in issues:
                tipo = "PR" if item.pull_request else "Issue"
                response += f"- **[{tipo} #{item.number}]** {item.title} (Autor: {item.user.login})\n"
                response += f"  Enlace: {item.html_url}\n"
            
            return response
        except Exception as e:
            return f"Error al listar elementos: {str(e)}"

    async def create_repository_issue(
        self,
        title: str,
        body: str,
        repository: Optional[str] = None,
        __event_emitter__=None,
    ) -> str:
        """
        Crea un nuevo Issue en un repositorio.
        Úsalo para reportar bugs, documentar ideas o asignar tareas pendientes.
        
        :param title: El título descriptivo del issue.
        :param body: El cuerpo o descripción detallada del issue. Soporta Markdown.
        :param repository: (Opcional) Repositorio en formato 'owner/repo'.
        """
        try:
            if not self.valves.github_token:
                return "Error: Token de GitHub no configurado."
            
            repo_name = self._get_repo_name(repository)
            await self._emit_status(__event_emitter__, f"Creando issue en {repo_name}...")
            
            g = Github(self.valves.github_token)
            repo = g.get_repo(repo_name)
            issue = repo.create_issue(title=title, body=body)
            
            await self._emit_status(__event_emitter__, "¡Issue creado con éxito!", done=True)
            return f"**Issue creado exitosamente**\n- Número: #{issue.number}\n- Título: {issue.title}\n- URL: {issue.html_url}"
        except Exception as e:
            return f"Error al crear el issue: {str(e)}"
