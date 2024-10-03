# **Inference Phi-3-Vision in Local**

Phi-3-vision-128k-instruct는 Phi-3가 언어를 이해할 뿐만 아니라 시각적으로 세상을 볼 수 있게 합니다. Phi-3-vision-128k-instruct를 통해 OCR, 표 분석, 객체 인식, 이미지 설명 등 다양한 시각 문제를 해결할 수 있습니다. 이전에는 많은 데이터 훈련이 필요했던 작업들을 손쉽게 완료할 수 있습니다. 다음은 Phi-3-vision-128k-instruct가 인용한 관련 기술과 응용 시나리오입니다.

## **0. 준비**

사용하기 전에 다음의 Python 라이브러리가 설치되어 있는지 확인하세요 (Python 3.10+ 권장)


```bash

pip install transformers -U
pip install datasets -U
pip install torch -U

```

***CUDA 11.6+***를 사용하는 것이 좋으며, flatten을 설치하세요


```bash

pip install flash-attn --no-build-isolation

```

새 Notebook을 만드세요. 예제를 완료하기 위해 다음 내용을 먼저 작성하는 것이 좋습니다.


```python

from PIL import Image
import requests
import torch
from transformers import AutoModelForCausalLM
from transformers import AutoProcessor

model_id = "microsoft/Phi-3-vision-128k-instruct"

kwargs = {}
kwargs['torch_dtype'] = torch.bfloat16

processor = AutoProcessor.from_pretrained(model_id, trust_remote_code=True)
model = AutoModelForCausalLM.from_pretrained(model_id, trust_remote_code=True, torch_dtype="auto").cuda()

user_prompt = '<|user|>\n'
assistant_prompt = '<|assistant|>\n'
prompt_suffix = "<|end|>\n"

```

## **1. Phi-3-Vision으로 이미지 분석**

AI가 우리의 사진 내용을 분석하고 관련 설명을 제공할 수 있게 하려면


```python

prompt = f"{user_prompt}<|image_1|>\nCould you please introduce this stock to me?{prompt_suffix}{assistant_prompt}"


url = "https://g.foolcdn.com/editorial/images/767633/nvidiadatacenterrevenuefy2017tofy2024.png"

image = Image.open(requests.get(url, stream=True).raw)

inputs = processor(prompt, image, return_tensors="pt").to("cuda:0")

generate_ids = model.generate(**inputs, 
                              max_new_tokens=1000,
                              eos_token_id=processor.tokenizer.eos_token_id,
                              )
generate_ids = generate_ids[:, inputs['input_ids'].shape[1]:]

response = processor.batch_decode(generate_ids, 
                                  skip_special_tokens=True, 
                                  clean_up_tokenization_spaces=False)[0]

```

Notebook에서 다음 스크립트를 실행하여 관련 답변을 얻을 수 있습니다


```txt

Certainly! Nvidia Corporation is a global leader in advanced computing and artificial intelligence (AI). The company designs and develops graphics processing units (GPUs), which are specialized hardware accelerators used to process and render images and video. Nvidia's GPUs are widely used in professional visualization, data centers, and gaming. The company also provides software and services to enhance the capabilities of its GPUs. Nvidia's innovative technologies have applications in various industries, including automotive, healthcare, and entertainment. The company's stock is publicly traded and can be found on major stock exchanges.

```

## **2. Phi-3-Vision으로 OCR**

이미지 분석 외에도 이미지를 통해 정보를 추출할 수 있습니다. 이는 복잡한 코드를 작성해야 했던 OCR 과정을 간소화합니다.


```python

prompt = f"{user_prompt}<|image_1|>\nHelp me get the title and author information of this book?{prompt_suffix}{assistant_prompt}"

url = "https://marketplace.canva.com/EAFPHUaBrFc/1/0/1003w/canva-black-and-white-modern-alone-story-book-cover-QHBKwQnsgzs.jpg"

image = Image.open(requests.get(url, stream=True).raw)

inputs = processor(prompt, image, return_tensors="pt").to("cuda:0")

generate_ids = model.generate(**inputs, 
                              max_new_tokens=1000,
                              eos_token_id=processor.tokenizer.eos_token_id,
                              )

generate_ids = generate_ids[:, inputs['input_ids'].shape[1]:]

response = processor.batch_decode(generate_ids, 
                                  skip_special_tokens=False, 
                                  clean_up_tokenization_spaces=False)[0]

```

결과는


```txt

The title of the book is "ALONE" and the author is Morgan Maxwell.

```

## **3. 여러 이미지 비교**

Phi-3 Vision은 여러 이미지를 비교하는 기능을 지원합니다. 이 모델을 사용하여 이미지 간의 차이점을 찾을 수 있습니다.


```python

prompt = f"{user_prompt}<|image_1|>\n<|image_2|>\n What is difference in this two images?{prompt_suffix}{assistant_prompt}"

print(f">>> Prompt\n{prompt}")

url = "https://hinhnen.ibongda.net/upload/wallpaper/doi-bong/2012/11/22/arsenal-wallpaper-free.jpg"

image_1 = Image.open(requests.get(url, stream=True).raw)

url = "https://assets-webp.khelnow.com/d7293de2fa93b29528da214253f1d8d0/news/uploads/2021/07/Arsenal-1024x576.jpg.webp"

image_2 = Image.open(requests.get(url, stream=True).raw)

images = [image_1, image_2]

inputs = processor(prompt, images, return_tensors="pt").to("cuda:0")

generate_ids = model.generate(**inputs, 
                              max_new_tokens=1000,
                              eos_token_id=processor.tokenizer.eos_token_id,
                              )

generate_ids = generate_ids[:, inputs['input_ids'].shape[1]:]

response = processor.batch_decode(generate_ids, skip_special_tokens=True, clean_up_tokenization_spaces=False)[0]

```

결과는


```txt

The first image shows a group of soccer players from the Arsenal Football Club posing for a team photo with their trophies, while the second image shows a group of soccer players from the Arsenal Football Club celebrating a victory with a large crowd of fans in the background. The difference between the two images is the context in which the photos were taken, with the first image focusing on the team and their trophies, and the second image capturing a moment of celebration and victory.

```

면책 조항: 이 번역은 원본을 AI 모델에 의해 번역된 것이며 완벽하지 않을 수 있습니다. 
출력을 검토하시고 필요한 수정 사항을 반영해 주시기 바랍니다.