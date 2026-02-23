# Embeddings para LLMs

Notebook educativo que explica cómo funcionan los embeddings en modelos de lenguaje y el impacto de los parámetros `max_length` y `stride`.

## Instalación

```bash
pip install torch tiktoken matplotlib pandas requests
```

## Paso a Paso del Notebook

### 1. Importar Librerías
Cargamos las herramientas necesarias: PyTorch, tiktoken, matplotlib y pandas.

### 2. Cargar el Texto
Descargamos y leemos "The Verdict" de Edith Wharton (~5,470 palabras).

### 3. Tokenización
Convertimos el texto en números (tokens) usando el tokenizador de GPT-2.
- Ejemplo: "Hello world" → [15496, 995]

### 4. Dataset con Ventana Deslizante
Creamos una clase que divide el texto en ventanas:
- **max_length**: Tamaño de cada ventana (ej: 16 tokens)
- **stride**: Cuánto se mueve la ventana (ej: 8 tokens)
- **overlap**: Cuántos tokens se repiten = max_length - stride

### 5. Embeddings
Convertimos los tokens en vectores de números:
- **Token embeddings**: Representación del token
- **Positional embeddings**: Posición del token en la secuencia
- Sumamos ambos para obtener el embedding final

## Los Experimentos

### Experimento 1: ¿Qué pasa si cambio max_length?

Probamos diferentes tamaños de ventana (4, 8, 16, 32, 64, 128 tokens):

**Resultado**: Ventanas más grandes → Menos muestras

| Tamaño ventana | Muestras |
|----------------|----------|
| 4 tokens       | 1,368    |
| 16 tokens      | 342      |
| 128 tokens     | 42       |

**¿Por qué?** Si cada ventana cubre más texto, necesitas menos ventanas para cubrir todo.

### Experimento 2: ¿Qué pasa si cambio stride?

Mantenemos max_length = 16 y probamos diferentes pasos (1, 2, 4, 8, 16):

**Resultado**: Pasos pequeños → Más overlap → Más muestras

| Stride | Muestras | Overlap |
|--------|----------|---------|
| 1      | 5,470    | 94%     |
| 8      | 684      | 50%     |
| 16     | 342      | 0%      |

**¿Por qué?** Si la ventana se mueve poco, se repite mucho contenido y genera más muestras.

## ¿Qué es el Overlap y por qué es útil?

**Overlap** = tokens que se repiten entre ventanas consecutivas

**Ventajas del overlap:**
- No se pierde contexto entre ventanas
- Más ejemplos de entrenamiento
- El modelo ve palabras en diferentes contextos

**Desventajas:**
- Más tiempo de entrenamiento
- Puede causar memorización excesiva

---

Creado para FDSI - Universidad (2026)
