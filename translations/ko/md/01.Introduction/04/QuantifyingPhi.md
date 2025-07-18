<!--
CO_OP_TRANSLATOR_METADATA:
{
  "original_hash": "d658062de70b131ef4c0bff69b5fc70e",
  "translation_date": "2025-07-16T21:43:39+00:00",
  "source_file": "md/01.Introduction/04/QuantifyingPhi.md",
  "language_code": "ko"
}
-->
# **Phi 패밀리 양자화**

모델 양자화는 신경망 모델의 파라미터(가중치 및 활성화 값 등)를 큰 값 범위(보통 연속적인 값 범위)에서 더 작은 유한 값 범위로 매핑하는 과정을 말합니다. 이 기술은 모델의 크기와 계산 복잡도를 줄이고, 모바일 기기나 임베디드 시스템과 같은 자원 제한 환경에서 모델의 운영 효율성을 높일 수 있습니다. 모델 양자화는 파라미터의 정밀도를 낮춰 압축을 달성하지만, 이로 인해 일정 수준의 정밀도 손실이 발생합니다. 따라서 양자화 과정에서는 모델 크기, 계산 복잡도, 정밀도 간의 균형을 맞추는 것이 중요합니다. 일반적인 양자화 방법으로는 고정 소수점 양자화, 부동 소수점 양자화 등이 있으며, 구체적인 상황과 필요에 따라 적절한 양자화 전략을 선택할 수 있습니다.

우리는 GenAI 모델을 엣지 디바이스에 배포하여 모바일 기기, AI PC/Copilot+PC, 전통적인 IoT 기기 등 더 많은 장치가 GenAI 시나리오에 참여할 수 있도록 하고자 합니다. 양자화 모델을 통해 다양한 디바이스에 맞춰 배포할 수 있으며, 하드웨어 제조사가 제공하는 모델 가속 프레임워크 및 양자화 모델과 결합하여 더 나은 SLM 애플리케이션 시나리오를 구축할 수 있습니다.

양자화 시나리오에서는 INT4, INT8, FP16, FP32 등 다양한 정밀도를 사용합니다. 아래는 일반적으로 사용되는 양자화 정밀도에 대한 설명입니다.

### **INT4**

INT4 양자화는 모델의 가중치와 활성화 값을 4비트 정수로 양자화하는 극단적인 방법입니다. 표현 범위가 작고 정밀도가 낮아 정밀도 손실이 더 크게 발생하는 경향이 있습니다. 하지만 INT8 양자화에 비해 저장 공간과 계산 복잡도를 더욱 줄일 수 있습니다. 다만, INT4 양자화는 정확도가 너무 낮아 모델 성능 저하가 심할 수 있어 실제 적용 사례는 드뭅니다. 또한 모든 하드웨어가 INT4 연산을 지원하지 않으므로, 양자화 방식을 선택할 때 하드웨어 호환성을 반드시 고려해야 합니다.

### **INT8**

INT8 양자화는 모델의 가중치와 활성화 값을 부동 소수점에서 8비트 정수로 변환하는 과정입니다. INT8 정수가 표현하는 수치 범위는 작고 정밀도는 낮지만, 저장 공간과 계산 요구량을 크게 줄일 수 있습니다. INT8 양자화에서는 모델의 가중치와 활성화 값이 스케일링과 오프셋을 포함한 양자화 과정을 거쳐 원래의 부동 소수점 정보를 최대한 보존합니다. 추론 시에는 이 양자화된 값들이 다시 부동 소수점으로 역양자화되어 계산되고, 다음 단계에서는 다시 INT8로 양자화됩니다. 이 방법은 대부분의 응용에서 충분한 정확도를 유지하면서 높은 계산 효율성을 제공합니다.

### **FP16**

FP16 포맷, 즉 16비트 부동 소수점 수(float16)는 32비트 부동 소수점 수(float32)에 비해 메모리 사용량을 절반으로 줄여 대규모 딥러닝 응용에 큰 이점을 제공합니다. FP16 포맷을 사용하면 동일한 GPU 메모리 한도 내에서 더 큰 모델을 로드하거나 더 많은 데이터를 처리할 수 있습니다. 최신 GPU 하드웨어가 FP16 연산을 지원함에 따라, FP16 포맷 사용 시 계산 속도 향상도 기대할 수 있습니다. 다만 FP16은 정밀도가 낮아 수치 불안정성이나 정밀도 손실이 발생할 수 있다는 단점도 있습니다.

### **FP32**

FP32 포맷은 더 높은 정밀도를 제공하며 넓은 값 범위를 정확하게 표현할 수 있습니다. 복잡한 수학 연산이 필요하거나 고정밀 결과가 요구되는 경우 FP32 포맷이 선호됩니다. 하지만 높은 정밀도는 더 많은 메모리 사용과 긴 계산 시간을 의미합니다. 특히 모델 파라미터가 많고 데이터가 방대한 대규모 딥러닝 모델에서는 FP32 포맷이 GPU 메모리 부족이나 추론 속도 저하를 초래할 수 있습니다.

모바일 기기나 IoT 기기에서는 Phi-3.x 모델을 INT4로 변환할 수 있으며, AI PC / Copilot PC는 INT8, FP16, FP32와 같은 더 높은 정밀도를 사용할 수 있습니다.

현재 다양한 하드웨어 제조사들이 Intel의 OpenVINO, Qualcomm의 QNN, Apple의 MLX, Nvidia의 CUDA 등 생성 모델을 지원하는 각기 다른 프레임워크를 제공하며, 모델 양자화와 결합해 로컬 배포를 완성하고 있습니다.

기술적으로는 PyTorch / Tensorflow 포맷, GGUF, ONNX 등 양자화 후 다양한 포맷 지원이 있습니다. 저는 GGUF와 ONNX 간의 포맷 비교 및 적용 시나리오를 진행했으며, 모델 프레임워크부터 하드웨어까지 폭넓게 지원하는 ONNX 양자화 포맷을 추천합니다. 이 장에서는 ONNX Runtime for GenAI, OpenVINO, Apple MLX를 중심으로 모델 양자화를 다룰 예정입니다(더 좋은 방법이 있다면 PR 제출을 통해 알려주세요).

**이 장의 내용**

1. [llama.cpp를 사용한 Phi-3.5 / 4 양자화](./UsingLlamacppQuantifyingPhi.md)

2. [onnxruntime용 Generative AI 확장 기능을 사용한 Phi-3.5 / 4 양자화](./UsingORTGenAIQuantifyingPhi.md)

3. [Intel OpenVINO를 사용한 Phi-3.5 / 4 양자화](./UsingIntelOpenVINOQuantifyingPhi.md)

4. [Apple MLX 프레임워크를 사용한 Phi-3.5 / 4 양자화](./UsingAppleMLXQuantifyingPhi.md)

**면책 조항**:  
이 문서는 AI 번역 서비스 [Co-op Translator](https://github.com/Azure/co-op-translator)를 사용하여 번역되었습니다. 정확성을 위해 노력하고 있으나, 자동 번역에는 오류나 부정확한 부분이 있을 수 있음을 유의하시기 바랍니다. 원문은 해당 언어의 원본 문서가 권위 있는 출처로 간주되어야 합니다. 중요한 정보의 경우 전문적인 인간 번역을 권장합니다. 본 번역 사용으로 인해 발생하는 오해나 잘못된 해석에 대해 당사는 책임을 지지 않습니다.