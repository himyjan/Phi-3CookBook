<!--
CO_OP_TRANSLATOR_METADATA:
{
  "original_hash": "3139a6a82f357a9f90f1fe51c4caf65a",
  "translation_date": "2025-05-07T10:46:27+00:00",
  "source_file": "md/01.Introduction/04/UsingIntelOpenVINOQuantifyingPhi.md",
  "language_code": "es"
}
-->
# **Cuantificación de Phi-3.5 usando Intel OpenVINO**

Intel es el fabricante de CPU más tradicional con muchos usuarios. Con el auge del aprendizaje automático y el aprendizaje profundo, Intel también se ha sumado a la competencia por la aceleración de IA. Para la inferencia de modelos, Intel no solo utiliza GPUs y CPUs, sino también NPUs.

Esperamos desplegar la familia Phi-3.x en el extremo, con la intención de convertirse en la parte más importante del PC de IA y el PC Copilot. La carga del modelo en el extremo depende de la cooperación de diferentes fabricantes de hardware. Este capítulo se centra principalmente en el escenario de aplicación de Intel OpenVINO como modelo cuantitativo.

## **Qué es OpenVINO**

OpenVINO es un kit de herramientas de código abierto para optimizar y desplegar modelos de aprendizaje profundo desde la nube hasta el edge. Acelera la inferencia de aprendizaje profundo en diversos casos de uso, como IA generativa, video, audio y lenguaje, con modelos de frameworks populares como PyTorch, TensorFlow, ONNX y más. Convierte y optimiza modelos, y despliega a través de una combinación de hardware y entornos Intel®, tanto on-premises como en el dispositivo, en el navegador o en la nube.

Ahora con OpenVINO, puedes cuantificar rápidamente el modelo GenAI en hardware Intel y acelerar la referencia del modelo.

Actualmente OpenVINO soporta la conversión de cuantificación de Phi-3.5-Vision y Phi-3.5 Instruct.

### **Configuración del entorno**

Por favor, asegúrate de tener instaladas las siguientes dependencias de entorno, este es requirement.txt

```txt

--extra-index-url https://download.pytorch.org/whl/cpu
optimum-intel>=1.18.2
nncf>=2.11.0
openvino>=2024.3.0
transformers>=4.40
openvino-genai>=2024.3.0.0

```

### **Cuantificación de Phi-3.5-Instruct usando OpenVINO**

En la terminal, ejecuta este script

```bash


export llm_model_id = "microsoft/Phi-3.5-mini-instruct"

export llm_model_path = "your save quantizing Phi-3.5-instruct location"

optimum-cli export openvino --model {llm_model_id} --task text-generation-with-past --weight-format int4 --group-size 128 --ratio 0.6  --sym  --trust-remote-code {llm_model_path}


```

### **Cuantificación de Phi-3.5-Vision usando OpenVINO**

Ejecuta este script en Python o Jupyter lab

```python

import requests
from pathlib import Path
from ov_phi3_vision import convert_phi3_model
import nncf

if not Path("ov_phi3_vision.py").exists():
    r = requests.get(url="https://raw.githubusercontent.com/openvinotoolkit/openvino_notebooks/latest/notebooks/phi-3-vision/ov_phi3_vision.py")
    open("ov_phi3_vision.py", "w").write(r.text)


if not Path("gradio_helper.py").exists():
    r = requests.get(url="https://raw.githubusercontent.com/openvinotoolkit/openvino_notebooks/latest/notebooks/phi-3-vision/gradio_helper.py")
    open("gradio_helper.py", "w").write(r.text)

if not Path("notebook_utils.py").exists():
    r = requests.get(url="https://raw.githubusercontent.com/openvinotoolkit/openvino_notebooks/latest/utils/notebook_utils.py")
    open("notebook_utils.py", "w").write(r.text)



model_id = "microsoft/Phi-3.5-vision-instruct"
out_dir = Path("../model/phi-3.5-vision-128k-instruct-ov")
compression_configuration = {
    "mode": nncf.CompressWeightsMode.INT4_SYM,
    "group_size": 64,
    "ratio": 0.6,
}
if not out_dir.exists():
    convert_phi3_model(model_id, out_dir, compression_configuration)

```

### **🤖 Ejemplos para Phi-3.5 con Intel OpenVINO**

| Laboratorios    | Introducción | Ir |
| -------- | ------- |  ------- |
| 🚀 Lab-Introduce Phi-3.5 Instruct  | Aprende a usar Phi-3.5 Instruct en tu PC de IA    |  [Ir](../../../../../code/09.UpdateSamples/Aug/intel-phi35-instruct-zh.ipynb)    |
| 🚀 Lab-Introduce Phi-3.5 Vision (imagen) | Aprende a usar Phi-3.5 Vision para analizar imágenes en tu PC de IA      |  [Ir](../../../../../code/09.UpdateSamples/Aug/intel-phi35-vision-img.ipynb)    |
| 🚀 Lab-Introduce Phi-3.5 Vision (video)   | Aprende a usar Phi-3.5 Vision para analizar video en tu PC de IA    |  [Ir](../../../../../code/09.UpdateSamples/Aug/intel-phi35-vision-video.ipynb)    |

## **Recursos**

1. Aprende más sobre Intel OpenVINO [https://www.intel.com/content/www/us/en/developer/tools/openvino-toolkit/overview.html](https://www.intel.com/content/www/us/en/developer/tools/openvino-toolkit/overview.html)

2. Repositorio GitHub de Intel OpenVINO [https://github.com/openvinotoolkit/openvino.genai](https://github.com/openvinotoolkit/openvino.genai)

**Aviso Legal**:  
Este documento ha sido traducido utilizando el servicio de traducción automática [Co-op Translator](https://github.com/Azure/co-op-translator). Aunque nos esforzamos por la precisión, tenga en cuenta que las traducciones automáticas pueden contener errores o inexactitudes. El documento original en su idioma nativo debe considerarse la fuente autorizada. Para información crítica, se recomienda una traducción profesional realizada por humanos. No nos hacemos responsables por malentendidos o interpretaciones erróneas derivadas del uso de esta traducción.