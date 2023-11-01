# openai
当使用GitHub API来获取代码库信息，然后使用Python中的词云库生成词云图
import requests
from wordcloud import WordCloud
import matplotlib.pyplot as plt

# 使用GitHub API获取代码库信息
def get_repository_info(repository):
    url = f"https://api.github.com/repos/{repository}"
    response = requests.get(url)
    if response.status_code == 200:
        data = response.json()
        description = data.get("description", "")
        topics = data.get("topics", [])
        return description, topics
    else:
        print("Failed to fetch repository information.")
        return None, None

# 生成词云图
def generate_wordcloud(text):
    wordcloud = WordCloud(width=800, height=400, background_color='white').generate(text)
    plt.figure(figsize=(10, 5))
    plt.imshow(wordcloud, interpolation='bilinear')
    plt.axis('off')
    plt.show()

# 输入GitHub代码库名称
repository_name = input("Enter the GitHub repository name (e.g., owner/repository): ")

# 获取代码库信息
description, topics = get_repository_info(repository_name)

# 打印代码库描述和主题
print("Repository Description:", description)
print("Repository Topics:", topics)

# 将描述和主题合并成一个字符串
text = description + " " + " ".join(topics)

# 生成词云图
generate_wordcloud(text)
