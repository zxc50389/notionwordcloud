import requests
from wordcloud import WordCloud
import matplotlib.pyplot as plt

# 定義 Notion API 相關參數
NOTION_API_KEY = "your_notion_api_key"
NOTION_DATABASE_ID = "your_notion_database_id"
MULTI_SELECT_PROPERTY_NAME = "your_multi_select_property_name"

# 從 Notion 獲取數據的函數
def get_notion_data():
    url = f"https://api.notion.com/v1/databases/{NOTION_DATABASE_ID}/query"
    headers = {
        "Authorization": f"Bearer {NOTION_API_KEY}",
        "Notion-Version": "2022-06-28",
        "Content-Type": "application/json"
    }
    response = requests.post(url, headers=headers)
    response.raise_for_status()
    return response.json()

# 處理數據並生成文字雲的函數
def generate_word_cloud(data):
    multi_select_counts = {}
    for result in data["results"]:
        properties = result["properties"]
        if MULTI_SELECT_PROPERTY_NAME in properties:
            multi_select_values = properties[MULTI_SELECT_PROPERTY_NAME]["multi_select"]
            for option in multi_select_values:
                name = option["name"]
                multi_select_counts[name] = multi_select_counts.get(name, 0) + 1

    # 生成文字雲
    wordcloud = WordCloud(width=800, height=400, background_color="white", font_path="simhei.ttf").generate_from_frequencies(multi_select_counts)
    plt.figure(figsize=(10, 5))
    plt.imshow(wordcloud, interpolation="bilinear")
    plt.axis("off")
    image_path = "wordcloud.png"
    plt.savefig(image_path)
    print(f"文字雲圖片已保存至 {image_path}")

# 主程序執行
if __name__ == "__main__":
    notion_data = get_notion_data()
    generate_word_cloud(notion_data)
