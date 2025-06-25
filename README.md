# 在那个 DeepSeek API 的代码示例中，你确实没有看到像 get_completion 这样的额外封装函数。那是因为这段 DeepSeek 代码已经足够简洁和直接，它本身就完成了与 API 交互的完整流程
```
# import requests

# API_KEY = "sk-6c36a52d4a8e4548b682e3c5bee0d148"  

# url = "https://api.deepseek.com/chat/completions"

# headers 参数通常是 API 提供商规定的，你必须按照他们的文档来设置，特别是认证信息和 Content-Type。
# headers = {
#     "Content-Type": "application/json", # 这是一个 Python 中的**字典（dict）**结构体。
#     "Authorization": f"Bearer {API_KEY}"
# }

# data 参数的内容是由你使用的特定 API（例如 DeepSeek 或 OpenAI 的聊天完成 API）的文档决定的。每个 API 会定义它接受哪些参数，以及这些参数的类型和含义。而参数中的具体值（比如 messages 里的 Prompt 内容、model 的选择、temperature 的值）则需要你根据你的应用程序逻辑和用户需求来动态设定。
# data = {
#     "model": "deepseek-reasoner",  # 指定使用 R1 模型（deepseek-reasoner）或者 V3 模型（deepseek-chat）
#     "messages": [
#         {"role": "system", "content": "你是一个专业的助手"},
#         {"role": "user", "content": "请解释量子计算的基本原理"}
#     ],
#     "stream": False  # 关闭流式传输
# }

# response = requests.post(url, headers=headers, json=data) # requests.post(...) 才是执行联网操作（发送 HTTP POST 请求到 API_URL）的部分。它会尝试连接到指定的服务器，并发送数据

# if response.status_code == 200:
#     result = response.json()
#     print(result['choices'][0]['message']['content'])
# else:
#     print("请求失败，错误码：", response.status_code)
```

# 相对于更复杂的 LLM 应用，比如那些需要“工具使用”或“多步规划”的 Agent 而言：这段 DeepSeek 代码确实没有涉及到辅助 LLM 进行复杂任务处理的函数
```
# import requests

# def call_deepseek_api(messages_content, model_name="deepseek-reasoner", temperature=0):
#     url = "https://api.deepseek.com/chat/completions"
#     API_KEY = "sk-6c36a52d4a8e4548b682e3c5bee0d148" 
# headers 主要负责请求的元信息和认证，而 data 则承载了请求的实际负载（比如你的 Prompt 和模型控制参数）。 
#     headers = {
#         "Content-Type": "application/json",
#         "Authorization": f"Bearer {API_KEY}" # API_KEY 需要在外部定义或作为参数传入
#     }
#     data = {
#         "model": model_name,
#         "messages": messages_content,
#         "stream": False
#     }
#     response = requests.post(url, headers=headers, json=data)
#     response.raise_for_status() # 如果请求失败，抛出异常
#     return response.json()['choices'][0]['message']['content']

# # 使用这个辅助函数
# my_prompt = [{"role": "user", "content": "帮我写一首关于夏天的诗"}]
# poem = call_deepseek_api(my_prompt)
# print(poem)

# my_other_prompt = [{"role": "user", "content": "用英文问候"}]
# greeting = call_deepseek_api(my_other_prompt, model_name="deepseek-chat", temperature=0.7)
# print(greeting)
```


# 多轮对话
```
# import requests

# def call_deepseek_api(messages_content, model_name="deepseek-reasoner", temperature=0):
#     url = "https://api.deepseek.com/chat/completions"
#     API_KEY = "sk-6c36a52d4a8e4548b682e3c5bee0d148" 
#     headers = {
#         "Content-Type": "application/json",
#         "Authorization": f"Bearer {API_KEY}" # API_KEY 需要在外部定义或作为参数传入
#     }
#     data = {
#         "model": model_name,
#         "messages": messages_content,
#         "stream": False
#     }
#     response = requests.post(url, headers=headers, json=data)
#     response.raise_for_status() # 如果请求失败，抛出异常
#     return response.json()['choices'][0]['message']['content']

# # 使用这个辅助函数
# my_prompt = [{"role": "system", "content": "你是一位诗人"},
#     {"role": "user", "content": "写一首关于春天的诗"},
#     {"role": "assistant", "content": "春风拂面柳丝长..."},
#     {"role": "user", "content": "请继续补充第二段"}]
# poem = call_deepseek_api(my_prompt)
# print(poem)
```



# # 流式传输模式就是 AI 模型在生成回复时，不是一次性给你最终答案，而是像水流一样，一点一点地把内容传输给你。 这种模式在构建流畅、响应迅速的 AI 应用（尤其是聊天界面）时非常有用。
```
# import requests

# def call_deepseek_api(messages_content, model_name="deepseek-reasoner", temperature=0):
#     url = "https://api.deepseek.com/chat/completions"
#     API_KEY = "sk-6c36a52d4a8e4548b682e3c5bee0d148" 
#     headers = {
#         "Content-Type": "application/json",
#         "Authorization": f"Bearer {API_KEY}" # API_KEY 需要在外部定义或作为参数传入
#     }
#     data = {
#         "model": model_name,
#         "messages": messages_content,
#         "stream": True
#     }
#     response = requests.post(url, headers=headers, json=data, stream=True)

#     # 流式传输模式替换原来的一次性输出模式
#     for line in response.iter_lines():
#         if line:
#             decoded_line = line.decode('utf-8')
#             print(decoded_line)

# # 使用这个辅助函数
# my_prompt = [{"role": "system", "content": "你是一位诗人"},
#     {"role": "user", "content": "写一首关于春天的诗"},
#     {"role": "assistant", "content": "春风拂面柳丝长..."},
#     {"role": "user", "content": "请继续补充第二段"}]
# poem = call_deepseek_api(my_prompt)
# print(poem)
```
