# Deploy InternVL2 Series

## LMDeploy

[LMDeploy](https://github.com/InternLM/lmdeploy) is a toolkit for compressing, deploying, and serving LLM, developed by the MMRazor and MMDeploy teams.

```sh
pip install lmdeploy==0.5.2
```

LMDeploy abstracts the complex inference process of multi-modal Vision-Language Models (VLM) into an easy-to-use pipeline, similar to the Large Language Model (LLM) inference pipeline.

### A 'Hello, world' example

`````{tabs}

````{tab} 2B

```python
from lmdeploy import pipeline, TurbomindEngineConfig, ChatTemplateConfig
from lmdeploy.vl import load_image

model = 'OpenGVLab/InternVL2-2B'
system_prompt = '我是书生·万象，英文名是InternVL，是由上海人工智能实验室及多家合作单位联合开发的多模态大语言模型。'
image = load_image('https://raw.githubusercontent.com/open-mmlab/mmdeploy/main/tests/data/tiger.jpeg')
chat_template_config = ChatTemplateConfig('internvl-internlm2')
chat_template_config.meta_instruction = system_prompt
pipe = pipeline(model, chat_template_config=chat_template_config,
                backend_config=TurbomindEngineConfig(session_len=8192))
response = pipe(('describe this image', image))
print(response.text)
```

````

````{tab} 4B

```python
from lmdeploy import pipeline, PytorchEngineConfig, ChatTemplateConfig
from lmdeploy.vl import load_image

model = 'OpenGVLab/InternVL2-4B'
system_prompt = '我是书生·万象，英文名是InternVL，是由上海人工智能实验室及多家合作单位联合开发的多模态大语言模型。'
image = load_image('https://raw.githubusercontent.com/open-mmlab/mmdeploy/main/tests/data/tiger.jpeg')
chat_template_config = ChatTemplateConfig('internvl-phi3')
chat_template_config.meta_instruction = system_prompt
pipe = pipeline(model, chat_template_config=chat_template_config,
                backend_config=PytorchEngineConfig(session_len=8192))
response = pipe(('describe this image', image))
print(response.text)
```

````

````{tab} 8B

```python
from lmdeploy import pipeline, TurbomindEngineConfig, ChatTemplateConfig
from lmdeploy.vl import load_image

model = 'OpenGVLab/InternVL2-8B'
system_prompt = '我是书生·万象，英文名是InternVL，是由上海人工智能实验室及多家合作单位联合开发的多模态大语言模型。'
image = load_image('https://raw.githubusercontent.com/open-mmlab/mmdeploy/main/tests/data/tiger.jpeg')
chat_template_config = ChatTemplateConfig('internvl-internlm2')
chat_template_config.meta_instruction = system_prompt
pipe = pipeline(model, chat_template_config=chat_template_config,
                backend_config=TurbomindEngineConfig(session_len=8192))
response = pipe(('describe this image', image))
print(response.text)
```

````

````{tab} 26B

```python
from lmdeploy import pipeline, TurbomindEngineConfig, ChatTemplateConfig
from lmdeploy.vl import load_image

model = 'OpenGVLab/InternVL2-26B'
system_prompt = '我是书生·万象，英文名是InternVL，是由上海人工智能实验室及多家合作单位联合开发的多模态大语言模型。'
image = load_image('https://raw.githubusercontent.com/open-mmlab/mmdeploy/main/tests/data/tiger.jpeg')
chat_template_config = ChatTemplateConfig('internvl-internlm2')
chat_template_config.meta_instruction = system_prompt
pipe = pipeline(model, chat_template_config=chat_template_config,
                backend_config=TurbomindEngineConfig(session_len=8192))
response = pipe(('describe this image', image))
print(response.text)
```

````

````{tab} 40B

```python
from lmdeploy import pipeline, TurbomindEngineConfig, ChatTemplateConfig
from lmdeploy.vl import load_image

model = 'OpenGVLab/InternVL2-40B'
system_prompt = '我是书生·万象，英文名是InternVL，是由上海人工智能实验室及多家合作单位联合开发的多模态大语言模型。'
image = load_image('https://raw.githubusercontent.com/open-mmlab/mmdeploy/main/tests/data/tiger.jpeg')
chat_template_config = ChatTemplateConfig('internvl-zh-hermes2')
chat_template_config.meta_instruction = system_prompt
pipe = pipeline(model, chat_template_config=chat_template_config,
                backend_config=TurbomindEngineConfig(session_len=8192))
response = pipe(('describe this image', image))
print(response.text)
```

````

````{tab} 76B

```python
Coming soon...
```

````

`````

If `ImportError` occurs while executing this case, please install the required dependency packages as prompted.

### Multi-images inference

When dealing with multiple images, you can put them all in one list. Keep in mind that multiple images will lead to a higher number of input tokens, and as a result, the size of the context window typically needs to be increased.

> Warning: Due to the scarcity of multi-image conversation data, the performance on multi-image tasks may be unstable, and it may require multiple attempts to achieve satisfactory results.

`````{tabs}

````{tab} 2B

```python
from lmdeploy import pipeline, TurbomindEngineConfig, ChatTemplateConfig
from lmdeploy.vl import load_image
from lmdeploy.vl.constants import IMAGE_TOKEN

model = 'OpenGVLab/InternVL2-2B'
system_prompt = '我是书生·万象，英文名是InternVL，是由上海人工智能实验室及多家合作单位联合开发的多模态大语言模型。'
chat_template_config = ChatTemplateConfig('internvl-internlm2')
chat_template_config.meta_instruction = system_prompt
pipe = pipeline(model, chat_template_config=chat_template_config,
                backend_config=TurbomindEngineConfig(session_len=8192))

image_urls=[
    'https://raw.githubusercontent.com/open-mmlab/mmdeploy/main/demo/resources/human-pose.jpg',
    'https://raw.githubusercontent.com/open-mmlab/mmdeploy/main/demo/resources/det.jpg'
]

images = [load_image(img_url) for img_url in image_urls]
# Numbering images improves multi-image conversations
response = pipe((f'Image-1: {IMAGE_TOKEN}\nImage-2: {IMAGE_TOKEN}\ndescribe these two images', images))
print(response.text)
```

````

````{tab} 4B

```python
from lmdeploy import pipeline, PytorchEngineConfig, ChatTemplateConfig
from lmdeploy.vl import load_image
from lmdeploy.vl.constants import IMAGE_TOKEN

model = 'OpenGVLab/InternVL2-4B'
system_prompt = '我是书生·万象，英文名是InternVL，是由上海人工智能实验室及多家合作单位联合开发的多模态大语言模型。'
chat_template_config = ChatTemplateConfig('internvl-phi3')
chat_template_config.meta_instruction = system_prompt
pipe = pipeline(model, chat_template_config=chat_template_config,
                backend_config=PytorchEngineConfig(session_len=8192))

image_urls=[
    'https://raw.githubusercontent.com/open-mmlab/mmdeploy/main/demo/resources/human-pose.jpg',
    'https://raw.githubusercontent.com/open-mmlab/mmdeploy/main/demo/resources/det.jpg'
]

images = [load_image(img_url) for img_url in image_urls]
# Numbering images improves multi-image conversations
response = pipe((f'Image-1: {IMAGE_TOKEN}\nImage-2: {IMAGE_TOKEN}\ndescribe these two images', images))
print(response.text)
```

````

````{tab} 8B

```python
from lmdeploy import pipeline, TurbomindEngineConfig, ChatTemplateConfig
from lmdeploy.vl import load_image
from lmdeploy.vl.constants import IMAGE_TOKEN

model = 'OpenGVLab/InternVL2-8B'
system_prompt = '我是书生·万象，英文名是InternVL，是由上海人工智能实验室及多家合作单位联合开发的多模态大语言模型。'
chat_template_config = ChatTemplateConfig('internvl-internlm2')
chat_template_config.meta_instruction = system_prompt
pipe = pipeline(model, chat_template_config=chat_template_config,
                backend_config=TurbomindEngineConfig(session_len=8192))

image_urls=[
    'https://raw.githubusercontent.com/open-mmlab/mmdeploy/main/demo/resources/human-pose.jpg',
    'https://raw.githubusercontent.com/open-mmlab/mmdeploy/main/demo/resources/det.jpg'
]

images = [load_image(img_url) for img_url in image_urls]
# Numbering images improves multi-image conversations
response = pipe((f'Image-1: {IMAGE_TOKEN}\nImage-2: {IMAGE_TOKEN}\ndescribe these two images', images))
print(response.text)
```

````

````{tab} 26B

```python
from lmdeploy import pipeline, TurbomindEngineConfig, ChatTemplateConfig
from lmdeploy.vl import load_image
from lmdeploy.vl.constants import IMAGE_TOKEN

model = 'OpenGVLab/InternVL2-26B'
system_prompt = '我是书生·万象，英文名是InternVL，是由上海人工智能实验室及多家合作单位联合开发的多模态大语言模型。'
chat_template_config = ChatTemplateConfig('internvl-internlm2')
chat_template_config.meta_instruction = system_prompt
pipe = pipeline(model, chat_template_config=chat_template_config,
                backend_config=TurbomindEngineConfig(session_len=8192))

image_urls=[
    'https://raw.githubusercontent.com/open-mmlab/mmdeploy/main/demo/resources/human-pose.jpg',
    'https://raw.githubusercontent.com/open-mmlab/mmdeploy/main/demo/resources/det.jpg'
]

images = [load_image(img_url) for img_url in image_urls]
# Numbering images improves multi-image conversations
response = pipe((f'Image-1: {IMAGE_TOKEN}\nImage-2: {IMAGE_TOKEN}\ndescribe these two images', images))
print(response.text)
```

````

````{tab} 40B

```python
from lmdeploy import pipeline, TurbomindEngineConfig, ChatTemplateConfig
from lmdeploy.vl import load_image
from lmdeploy.vl.constants import IMAGE_TOKEN

model = 'OpenGVLab/InternVL2-40B'
system_prompt = '我是书生·万象，英文名是InternVL，是由上海人工智能实验室及多家合作单位联合开发的多模态大语言模型。'
chat_template_config = ChatTemplateConfig('internvl-zh-hermes2')
chat_template_config.meta_instruction = system_prompt
pipe = pipeline(model, chat_template_config=chat_template_config,
                backend_config=TurbomindEngineConfig(session_len=8192))

image_urls=[
    'https://raw.githubusercontent.com/open-mmlab/mmdeploy/main/demo/resources/human-pose.jpg',
    'https://raw.githubusercontent.com/open-mmlab/mmdeploy/main/demo/resources/det.jpg'
]

images = [load_image(img_url) for img_url in image_urls]
# Numbering images improves multi-image conversations
response = pipe((f'Image-1: {IMAGE_TOKEN}\nImage-2: {IMAGE_TOKEN}\ndescribe these two images', images))
print(response.text)
```

````

````{tab} 76B

```python
Coming soon...
```

````

`````

### Batch prompts inference

Conducting inference with batch prompts is quite straightforward; just place them within a list structure:

`````{tabs}

````{tab} 2B

```python
from lmdeploy import pipeline, TurbomindEngineConfig, ChatTemplateConfig
from lmdeploy.vl import load_image

model = 'OpenGVLab/InternVL2-2B'
system_prompt = '我是书生·万象，英文名是InternVL，是由上海人工智能实验室及多家合作单位联合开发的多模态大语言模型。'
chat_template_config = ChatTemplateConfig('internvl-internlm2')
chat_template_config.meta_instruction = system_prompt
pipe = pipeline(model, chat_template_config=chat_template_config,
                backend_config=TurbomindEngineConfig(session_len=8192))

image_urls=[
    "https://raw.githubusercontent.com/open-mmlab/mmdeploy/main/demo/resources/human-pose.jpg",
    "https://raw.githubusercontent.com/open-mmlab/mmdeploy/main/demo/resources/det.jpg"
]
prompts = [('describe this image', load_image(img_url)) for img_url in image_urls]
response = pipe(prompts)
print(response)
```

````

````{tab} 4B

```python
from lmdeploy import pipeline, PytorchEngineConfig, ChatTemplateConfig
from lmdeploy.vl import load_image

model = 'OpenGVLab/InternVL2-4B'
system_prompt = '我是书生·万象，英文名是InternVL，是由上海人工智能实验室及多家合作单位联合开发的多模态大语言模型。'
chat_template_config = ChatTemplateConfig('internvl-phi3')
chat_template_config.meta_instruction = system_prompt
pipe = pipeline(model, chat_template_config=chat_template_config,
                backend_config=PytorchEngineConfig(session_len=8192))

image_urls=[
    "https://raw.githubusercontent.com/open-mmlab/mmdeploy/main/demo/resources/human-pose.jpg",
    "https://raw.githubusercontent.com/open-mmlab/mmdeploy/main/demo/resources/det.jpg"
]
prompts = [('describe this image', load_image(img_url)) for img_url in image_urls]
response = pipe(prompts)
print(response)
```

````

````{tab} 8B

```python
from lmdeploy import pipeline, TurbomindEngineConfig, ChatTemplateConfig
from lmdeploy.vl import load_image

model = 'OpenGVLab/InternVL2-8B'
system_prompt = '我是书生·万象，英文名是InternVL，是由上海人工智能实验室及多家合作单位联合开发的多模态大语言模型。'
chat_template_config = ChatTemplateConfig('internvl-internlm2')
chat_template_config.meta_instruction = system_prompt
pipe = pipeline(model, chat_template_config=chat_template_config,
                backend_config=TurbomindEngineConfig(session_len=8192))

image_urls=[
    "https://raw.githubusercontent.com/open-mmlab/mmdeploy/main/demo/resources/human-pose.jpg",
    "https://raw.githubusercontent.com/open-mmlab/mmdeploy/main/demo/resources/det.jpg"
]
prompts = [('describe this image', load_image(img_url)) for img_url in image_urls]
response = pipe(prompts)
print(response)
```

````

````{tab} 26B

```python
from lmdeploy import pipeline, TurbomindEngineConfig, ChatTemplateConfig
from lmdeploy.vl import load_image

model = 'OpenGVLab/InternVL2-26B'
system_prompt = '我是书生·万象，英文名是InternVL，是由上海人工智能实验室及多家合作单位联合开发的多模态大语言模型。'
chat_template_config = ChatTemplateConfig('internvl-internlm2')
chat_template_config.meta_instruction = system_prompt
pipe = pipeline(model, chat_template_config=chat_template_config,
                backend_config=TurbomindEngineConfig(session_len=8192))

image_urls=[
    "https://raw.githubusercontent.com/open-mmlab/mmdeploy/main/demo/resources/human-pose.jpg",
    "https://raw.githubusercontent.com/open-mmlab/mmdeploy/main/demo/resources/det.jpg"
]
prompts = [('describe this image', load_image(img_url)) for img_url in image_urls]
response = pipe(prompts)
print(response)
```

````

````{tab} 40B

```python
from lmdeploy import pipeline, TurbomindEngineConfig, ChatTemplateConfig
from lmdeploy.vl import load_image

model = 'OpenGVLab/InternVL2-40B'
system_prompt = '我是书生·万象，英文名是InternVL，是由上海人工智能实验室及多家合作单位联合开发的多模态大语言模型。'
chat_template_config = ChatTemplateConfig('internvl-zh-hermes2')
chat_template_config.meta_instruction = system_prompt
pipe = pipeline(model, chat_template_config=chat_template_config,
                backend_config=TurbomindEngineConfig(session_len=8192))

image_urls=[
    "https://raw.githubusercontent.com/open-mmlab/mmdeploy/main/demo/resources/human-pose.jpg",
    "https://raw.githubusercontent.com/open-mmlab/mmdeploy/main/demo/resources/det.jpg"
]
prompts = [('describe this image', load_image(img_url)) for img_url in image_urls]
response = pipe(prompts)
print(response)
```

````

````{tab} 76B

```python
Coming soon...
```

````

`````

### Multi-turn conversation

There are two ways to do the multi-turn conversations with the pipeline. One is to construct messages according to the format of OpenAI and use above introduced method, the other is to use the `pipeline.chat` interface.

`````{tabs}

````{tab} 2B

```python
from lmdeploy import pipeline, TurbomindEngineConfig, ChatTemplateConfig, GenerationConfig
from lmdeploy.vl import load_image

model = 'OpenGVLab/InternVL2-2B'
system_prompt = '我是书生·万象，英文名是InternVL，是由上海人工智能实验室及多家合作单位联合开发的多模态大语言模型。'
chat_template_config = ChatTemplateConfig('internvl-internlm2')
chat_template_config.meta_instruction = system_prompt
pipe = pipeline(model, chat_template_config=chat_template_config,
                backend_config=TurbomindEngineConfig(session_len=8192))

image = load_image('https://raw.githubusercontent.com/open-mmlab/mmdeploy/main/demo/resources/human-pose.jpg')
gen_config = GenerationConfig(top_k=40, top_p=0.8, temperature=0.8)
sess = pipe.chat(('describe this image', image), gen_config=gen_config)
print(sess.response.text)
sess = pipe.chat('What is the woman doing?', session=sess, gen_config=gen_config)
print(sess.response.text)
```

````

````{tab} 4B

```python
from lmdeploy import pipeline, PytorchEngineConfig, ChatTemplateConfig, GenerationConfig
from lmdeploy.vl import load_image

model = 'OpenGVLab/InternVL2-4B'
system_prompt = '我是书生·万象，英文名是InternVL，是由上海人工智能实验室及多家合作单位联合开发的多模态大语言模型。'
chat_template_config = ChatTemplateConfig('internvl-phi3')
chat_template_config.meta_instruction = system_prompt
pipe = pipeline(model, chat_template_config=chat_template_config,
                backend_config=PytorchEngineConfig(session_len=8192))

image = load_image('https://raw.githubusercontent.com/open-mmlab/mmdeploy/main/demo/resources/human-pose.jpg')
gen_config = GenerationConfig(top_k=40, top_p=0.8, temperature=0.8)
sess = pipe.chat(('describe this image', image), gen_config=gen_config)
print(sess.response.text)
sess = pipe.chat('What is the woman doing?', session=sess, gen_config=gen_config)
print(sess.response.text)
```

````

````{tab} 8B

```python
from lmdeploy import pipeline, TurbomindEngineConfig, ChatTemplateConfig, GenerationConfig
from lmdeploy.vl import load_image

model = 'OpenGVLab/InternVL2-8B'
system_prompt = '我是书生·万象，英文名是InternVL，是由上海人工智能实验室及多家合作单位联合开发的多模态大语言模型。'
chat_template_config = ChatTemplateConfig('internvl-internlm2')
chat_template_config.meta_instruction = system_prompt
pipe = pipeline(model, chat_template_config=chat_template_config,
                backend_config=TurbomindEngineConfig(session_len=8192))

image = load_image('https://raw.githubusercontent.com/open-mmlab/mmdeploy/main/demo/resources/human-pose.jpg')
gen_config = GenerationConfig(top_k=40, top_p=0.8, temperature=0.8)
sess = pipe.chat(('describe this image', image), gen_config=gen_config)
print(sess.response.text)
sess = pipe.chat('What is the woman doing?', session=sess, gen_config=gen_config)
print(sess.response.text)
```

````

````{tab} 26B

```python
from lmdeploy import pipeline, TurbomindEngineConfig, ChatTemplateConfig, GenerationConfig
from lmdeploy.vl import load_image

model = 'OpenGVLab/InternVL2-26B'
system_prompt = '我是书生·万象，英文名是InternVL，是由上海人工智能实验室及多家合作单位联合开发的多模态大语言模型。'
chat_template_config = ChatTemplateConfig('internvl-internlm2')
chat_template_config.meta_instruction = system_prompt
pipe = pipeline(model, chat_template_config=chat_template_config,
                backend_config=TurbomindEngineConfig(session_len=8192))

image = load_image('https://raw.githubusercontent.com/open-mmlab/mmdeploy/main/demo/resources/human-pose.jpg')
gen_config = GenerationConfig(top_k=40, top_p=0.8, temperature=0.8)
sess = pipe.chat(('describe this image', image), gen_config=gen_config)
print(sess.response.text)
sess = pipe.chat('What is the woman doing?', session=sess, gen_config=gen_config)
print(sess.response.text)
```

````

````{tab} 40B

```python
from lmdeploy import pipeline, TurbomindEngineConfig, ChatTemplateConfig, GenerationConfig
from lmdeploy.vl import load_image

model = 'OpenGVLab/InternVL2-40B'
system_prompt = '我是书生·万象，英文名是InternVL，是由上海人工智能实验室及多家合作单位联合开发的多模态大语言模型。'
chat_template_config = ChatTemplateConfig('internvl-zh-hermes2')
chat_template_config.meta_instruction = system_prompt
pipe = pipeline(model, chat_template_config=chat_template_config,
                backend_config=TurbomindEngineConfig(session_len=8192))

image = load_image('https://raw.githubusercontent.com/open-mmlab/mmdeploy/main/demo/resources/human-pose.jpg')
gen_config = GenerationConfig(top_k=40, top_p=0.8, temperature=0.8)
sess = pipe.chat(('describe this image', image), gen_config=gen_config)
print(sess.response.text)
sess = pipe.chat('What is the woman doing?', session=sess, gen_config=gen_config)
print(sess.response.text)
```

````

````{tab} 76B

```python
Coming soon...
```

````

`````

### Serving with OpenAI Compatible Server

#### Launch Service

`````{tabs}

````{tab} 2B

LMDeploy's `api_server` enables models to be easily packed into services with a single command. The provided RESTful APIs are compatible with OpenAI's interfaces. Below are an example of service startup:

```shell
lmdeploy serve api_server OpenGVLab/InternVL2-2B --model-name InternVL2-2B --backend turbomind --server-port 23333
```

You can also load 4-bit AWQ quantized models to save memory:

```shell
lmdeploy serve api_server OpenGVLab/InternVL2-2B-AWQ --model-name InternVL2-2B --backend turbomind --server-port 23333 --model-format awq
```

The arguments of `api_server` can be viewed through the command `lmdeploy serve api_server -h`, for instance, `--tp` to set tensor parallelism, `--session-len` to specify the max length of the context window, `--cache-max-entry-count` to adjust the GPU mem ratio for k/v cache etc.
````

````{tab} 4B

> **⚠️ Warning**: This model only supports **Pytorch** backends for now.

LMDeploy's `api_server` enables models to be easily packed into services with a single command. The provided RESTful APIs are compatible with OpenAI's interfaces. Below are an example of service startup:

```shell
lmdeploy serve api_server OpenGVLab/InternVL2-4B --model-name InternVL2-4B --backend pytorch --server-port 23333
```

The arguments of `api_server` can be viewed through the command `lmdeploy serve api_server -h`, for instance, `--tp` to set tensor parallelism, `--session-len` to specify the max length of the context window, `--cache-max-entry-count` to adjust the GPU mem ratio for k/v cache etc.
````

````{tab} 8B

LMDeploy's `api_server` enables models to be easily packed into services with a single command. The provided RESTful APIs are compatible with OpenAI's interfaces. Below are an example of service startup:

```shell
lmdeploy serve api_server OpenGVLab/InternVL2-8B --model-name InternVL2-8B --backend turbomind --server-port 23333
```

You can also load 4-bit AWQ quantized models to save memory:

```shell
lmdeploy serve api_server OpenGVLab/InternVL2-8B-AWQ --model-name InternVL2-8B --backend turbomind --server-port 23333 --model-format awq
```

The arguments of `api_server` can be viewed through the command `lmdeploy serve api_server -h`, for instance, `--tp` to set tensor parallelism, `--session-len` to specify the max length of the context window, `--cache-max-entry-count` to adjust the GPU mem ratio for k/v cache etc.
````

````{tab} 26B

LMDeploy's `api_server` enables models to be easily packed into services with a single command. The provided RESTful APIs are compatible with OpenAI's interfaces. Below are an example of service startup:

```shell
lmdeploy serve api_server OpenGVLab/InternVL2-26B --model-name InternVL2-26B --backend turbomind --server-port 23333
```

You can also load 4-bit AWQ quantized models to save memory:

```shell
lmdeploy serve api_server OpenGVLab/InternVL2-26B-AWQ --model-name InternVL2-26B --backend turbomind --server-port 23333 --model-format awq
```

The arguments of `api_server` can be viewed through the command `lmdeploy serve api_server -h`, for instance, `--tp` to set tensor parallelism, `--session-len` to specify the max length of the context window, `--cache-max-entry-count` to adjust the GPU mem ratio for k/v cache etc.
````

````{tab} 40B

LMDeploy's `api_server` enables models to be easily packed into services with a single command. The provided RESTful APIs are compatible with OpenAI's interfaces. Below are an example of service startup:

```shell
lmdeploy serve api_server OpenGVLab/InternVL2-40B --model-name InternVL2-40B --backend turbomind --server-port 23333 --tp 2
```

You can also load 4-bit AWQ quantized models to save memory:

```shell
lmdeploy serve api_server OpenGVLab/InternVL2-40B-AWQ --model-name InternVL2-40B --backend turbomind --server-port 23333 --model-format awq
```

The arguments of `api_server` can be viewed through the command `lmdeploy serve api_server -h`, for instance, `--tp` to set tensor parallelism, `--session-len` to specify the max length of the context window, `--cache-max-entry-count` to adjust the GPU mem ratio for k/v cache etc.
````

````{tab} 76B

LMDeploy's `api_server` enables models to be easily packed into services with a single command. The provided RESTful APIs are compatible with OpenAI's interfaces. Below are an example of service startup:

```shell
lmdeploy serve api_server OpenGVLab/InternVL2-Llama3-76B --model-name InternVL2-Llama3-76B --backend turbomind --server-port 23333 --tp 4
```

You can also load 4-bit AWQ quantized models to save memory:

```shell
lmdeploy serve api_server OpenGVLab/InternVL2-Llama3-76B-AWQ --model-name InternVL2-Llama3-76B --backend turbomind --server-port 23333 --model-format awq
```

The arguments of `api_server` can be viewed through the command `lmdeploy serve api_server -h`, for instance, `--tp` to set tensor parallelism, `--session-len` to specify the max length of the context window, `--cache-max-entry-count` to adjust the GPU mem ratio for k/v cache etc.
````

`````

#### Integrate with `OpenAI`

Here is an example of interaction with the endpoint `v1/chat/completions` service via the openai package. Before running it, please install the openai package by `pip install openai`.

```python
from openai import OpenAI

client = OpenAI(api_key='YOUR_API_KEY', base_url='http://0.0.0.0:23333/v1')
model_name = client.models.list().data[0].id
response = client.chat.completions.create(
    model=model_name,
    messages=[{
        'role':
        'user',
        'content': [{
            'type': 'text',
            'text': 'describe this image',
        }, {
            'type': 'image_url',
            'image_url': {
                'url':
                'https://modelscope.oss-cn-beijing.aliyuncs.com/resource/tiger.jpeg',
            },
        }],
    }],
    temperature=0.8,
    top_p=0.8)
print(response)
```

If you encounter any issues or need advanced usage with `lmdeploy`, we recommend reading the [lmdeploy documentation](https://lmdeploy.readthedocs.io/en/latest/serving/api_server_vl.html).

#### Memory Usage Testing

To test the memory usage with several A100 GPUs, we will consider the following variables: the number of GPUs, whether AWQ 4-bit quantization is used, and the size of `--cache-max-entry-count`. The table below shows the memory usage per GPU under different scenarios:

`````{tabs}

````{tab} 2B

| #GPUs | AWQ 4-bit | cache-max-entry-count | Memory Usage per GPU |
| :---: | :-------: | :-------------------: | :------------------: |
|   1   |    No     |          0.8          |      67140 MB        |
|   1   |    No     |          0.2          |      21284 MB        |
|   1   |    No     |          0.1          |      13700 MB        |
|   1   |    No     |          0.05         |      9860 MB        |
|   1   |    Yes    |          0.05         |      7850 MB        |
|   2   |    No     |          0.05         |      8612 MB        |
|   4   |    No     |          0.05         |      7916 MB        |

````

````{tab} 4B

| #GPUs | AWQ 4-bit | cache-max-entry-count | Memory Usage per GPU |
| :---: | :-------: | :-------------------: | :------------------: |
|   1   |    No     |          0.8          |      67666 MB        |
|   1   |    No     |          0.2          |      24914 MB        |
|   1   |    No     |          0.1          |      17746 MB        |
|   1   |    No     |          0.05         |      14162 MB        |
|   2   |    No     |          0.05         |      11700 MB        |
|   4   |    No     |          0.05         |      10216 MB        |

````

````{tab} 8B

| #GPUs | AWQ 4-bit | cache-max-entry-count | Memory Usage per GPU |
| :---: | :-------: | :-------------------: | :------------------: |
|   1   |    No     |          0.8          |      69708 MB        |
|   1   |    No     |          0.2          |      30624 MB        |
|   1   |    No     |          0.1          |      24108 MB        |
|   1   |    No     |          0.05         |      20832 MB        |
|   1   |    Yes     |         0.05         |      11528 MB        |
|   2   |    No     |          0.05         |      14570 MB        |
|   4   |    No     |          0.05         |      11378 MB        |

````

````{tab} 26B

> **⚠️ Warning:** It seems InternViT-6B still has some bugs working with `--tp`.

| #GPUs | AWQ 4-bit | cache-max-entry-count | Memory Usage per GPU |
| :---: | :-------: | :-------------------: | :------------------: |
|   1   |    No     |          0.8          |       77310 MB       |
|   1   |    No     |          0.2          |       58302 MB       |
|   1   |    Yes    |          0.8          |       72104 MB       |
|   1   |    Yes    |          0.2          |       37448 MB       |
|   1   |    Yes    |          0.1          |       31656 MB       |
|   1   |    Yes    |          0.05         |       28712 MB       |
|   2   |    Yes    |          0.2          |     `CUDA error`     |
|   4   |    Yes    |          0.2          |     `CUDA error`     |
|   8   |    Yes    |          0.2          |     `CUDA error`     |

````

````{tab} 40B

> **⚠️ Warning:** It seems InternViT-6B still has some bugs working with `--tp`.

| #GPUs | AWQ 4-bit | cache-max-entry-count | Memory Usage per GPU |
| :---: | :-------: | :-------------------: | :------------------: |
|   1   |    No     |          0.8          |   `Out-Of-Memory`    |
|   1   |    No     |          0.2          |       79892 MB       |
|   1   |    No     |          0.1          |      `No output`     |
|   1   |    No     |          0.05         |      `No output`     |
|   1   |    Yes    |          0.8          |       72964 MB       |
|   1   |    Yes    |          0.2          |       42628 MB       |
|   1   |    Yes    |          0.1          |       37572 MB       |
|   1   |    Yes    |          0.05         |       33636 MB       |
|   2   |    Yes    |          0.2          |     `CUDA error`     |
|   4   |    Yes    |          0.2          |     `CUDA error`     |
|   8   |    Yes    |          0.2          |     `CUDA error`     |

````

````{tab} 76B

> **⚠️ Warning:** It seems InternViT-6B still has some bugs working with `--tp`.

| #GPUs | AWQ 4-bit | cache-max-entry-count | Memory Usage per GPU |
| :---: | :-------: | :-------------------: | :------------------: |
|   1   |    No     |          0.8          |   `Out-Of-Memory`    |
|   1   |    No     |          0.2          |   `Out-Of-Memory`    |
|   1   |    Yes    |          0.8          |       77138 MB       |
|   1   |    Yes    |          0.2          |       58738 MB       |
|   1   |    Yes    |          0.1          |       55698 MB       |
|   1   |    Yes    |          0.05         |       54130 MB       |
|   2   |    Yes    |          0.2          |     `CUDA error`     |
|   4   |    Yes    |          0.2          |     `CUDA error`     |
|   8   |    Yes    |          0.2          |     `CUDA error`     |

````

`````

## vLLM

Coming soon…

## Ollama

Coming soon…

<br>
<br>