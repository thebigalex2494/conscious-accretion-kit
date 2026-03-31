---
description: "Estándares de desarrollo Python y generales para Central Command"
globs: "**/*.py"
---

# Development Standards

## Python Standards
- **PEP 8** estricto + type hints con sintaxis Python 3.10+ (`X | None`, no `Optional[X]`)
- **UTF-8** encoding en todo lugar (`io.TextIOWrapper` para Windows)
- **Raw strings** para Windows paths (`r'C:\Users\...'`) o `pathlib.Path`
- **DRY** — importar via `sys.path`, NUNCA duplicar código
- **Asyncio** para operaciones paralelas cuando aplique
- **Modularidad**: no código global — envolver en `class` o `main()`
- **Archivos <200 líneas** cuando sea posible (eficiencia de contexto LLM)

## Configuration
- Mover paths/tokens hardcoded a `config/settings.py` o `.env`
- Sin dead code, sin unused imports
- Excepciones específicas, nunca bare `except`

## Windows Compatibility
- Usar `pathlib` o raw strings para paths
- Considerar encoding UTF-8 explícito en file I/O

## Testing
- Tests requeridos para módulos de producción
- Usar `pytest` como framework estándar
- Naming: `test_{module}.py` con funciones `test_{behavior}`

## Code Quality
- Comentarios solo donde la lógica no es auto-explicativa
- Variables y funciones con nombres descriptivos en inglés
- Constants en UPPER_SNAKE_CASE
