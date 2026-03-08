
# ProyectoGranja

## Configuración del Entorno Virtual

```bash
# Crear el entorno virtual
python -m venv venv

# Activar el entorno virtual
# En Windows:
venv\Scripts\activate
# En Linux/Mac:
source venv/bin/activate
```

## Instalación de Dependencias

```bash
pip install -r requirements.txt
```

## Estructura del Proyecto

```
ProyectoGranja/
├── venv/
├── src/
├── tests/
├── requirements.txt
└── README.md
```

## Ejecutar la Aplicación

```bash
python -m src.main
```

## Pipeline CI/CD

### 1. Pruebas Unitarias
```bash
pytest tests/ -v
```

### 2. Linting
```bash
flake8 src/
```

### 3. Formateo de Código
```bash
black src/
```

### 4. Type Checking
```bash
mypy src/
```

## Ejecutar Pipeline Completo

```bash
# Instalar herramientas de desarrollo
pip install pytest flake8 black mypy

# Ejecutar todo
pytest && flake8 src/ && black src/ && mypy src/
```

## Requisitos

- Python 3.8+
- pip
