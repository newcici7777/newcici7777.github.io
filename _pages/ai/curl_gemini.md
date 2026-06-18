---
title: curl Gemini Api
date: 2026-06-18
keywords: curl, gemini Api
---
參考網址:<https://ai.google.dev/api?hl=zh-tw>
參考網址:<https://ai.google.dev/api/generate-content?hl=zh-tw>

## curl 語法
```
curl "網址" \
  -H "Header" \
  -H 'Content-Type: application/json' \
  -X POST \
  -d `傳送的Body`
```

- `-H` 為Header請求標頭
- Content-Type 為傳送的Body格式，可為text或json
- `-X` HTTP 請求方法，可以為POST或GET
- `-d` d為Data（資料）的縮寫，後面單引號 ' ' 裡面為資料主體（Payload）

以下body為文字對話，contents為對話內容，parts是list，text為文字，內容為提問的問題。
```
{
    "contents": [
      {
        "parts": [
          {
            "text": "Explain how AI works in a few words"
          }
        ]
      }
    ]
}
```

## curl Gemini Api
```
curl "https://generativelanguage.googleapis.com/v1beta/models/gemini-2.5-flash:generateContent" \
  -H "x-goog-api-key: 你的API KEY" \
  -H 'Content-Type: application/json' \
  -X POST \
  -d '{
    "contents": [
      {
        "parts": [
          {
            "text": "Explain how AI works in a few words"
          }
        ]
      }
    ]
  }'
```
回傳內容如下:<br>
- promptTokenCount 詢問消耗的Token數量
- candidatesTokenCount AI回答消耗的Token數量
```
{
  "candidates": [
    {
      "content": {
        "parts": [
          {
            "text": "AI learns from data to make predictions or decisions."
          }
        ],
        "role": "model"
      },
      "finishReason": "STOP",
      "index": 0
    }
  ],
  "usageMetadata": {
    "promptTokenCount": 8,
    "candidatesTokenCount": 10,
    "totalTokenCount": 579,
    "promptTokensDetails": [
      {
        "modality": "TEXT",
        "tokenCount": 8
      }
    ],
    "thoughtsTokenCount": 561,
    "serviceTier": "standard"
  },
  "modelVersion": "gemini-2.5-flash",
  "responseId": "xEIzaojQD-Cb1e8P7p-56Qw"
}
```

## Postman 測試API
Header:<br>
![img]({{site.imgurl}}/ai/post_man1.png)<br>
body:<br>
![img]({{site.imgurl}}/ai/post_man2.png)<br>

## 多次對話:
因為單次對話沒辦法記錄前一個問題，所以需使用多次對話記錄大模型之前的對話，model為記錄大模型之前的對話。<br>

- role為user 使用者提出問題
- role為model 記錄大模型的回答

body的內容如下:
```
-d '{
    "contents": [
      {
          "role": "user",
          "parts": [
              // A list of Part objects goes here
          ]
      },
      {
          "role": "model",
          "parts": [
              // A list of Part objects goes here
          ]
      }
    ]
  }'
```
contents 為 對話的內容。<br>
- role 為 user使用者 或 model 大模型。<br>
- parts 為 提問內容 或 大模型回覆內容。

多次對話的順序要為user -> model -> user -> model -> user <br>
最後一次對話一定要為user。<br>

以下範例為:<br>
第一次:<br>
user 問: 我有10個蘋果分給5個人怎麼分?<br>
大模型 回: 每個人2顆蘋果<br>

第二次:<br>
user 問: 那如果分成2個人呢？<br>

大模型就會根據之前的user問、model回，找出原本的問題`我有10個蘋果分給5個人怎麼分`，產生回答。<br>
大模型 回: 那每個人就是5顆蘋果。<br>

```
curl "https://generativelanguage.googleapis.com/v1beta/models/gemini-2.5-flash:generateContent" \
  -H "x-goog-api-key: 你的APIkey" \
  -H "Content-Type: application/json" \
  -X POST \
  -d '{
    "contents": [
      {
        "role": "user",
        "parts": [
          {
            "text": "我有10個蘋果分給5個人怎麼分?"
          }
        ]
      },
      {
        "role": "model",
        "parts": [
          {
            "text": "每個人2顆蘋果"
          }
        ]
      },
      {
        "role": "user",
        "parts": [
          {
            "text": "那如果分成2個人呢？"
          }
        ]
      }
    ]
  }'
```

大模型回覆內容:
```
{
    "candidates": [
        {
            "content": {
                "parts": [
                    {
                        "text": "那每個人就是5顆蘋果。"
                    }
                ],
                "role": "model"
            },
            "finishReason": "STOP",
            "index": 0
        }
    ],
    "usageMetadata": {
        "promptTokenCount": 27,
        "candidatesTokenCount": 8,
        "totalTokenCount": 273,
        "promptTokensDetails": [
            {
                "modality": "TEXT",
                "tokenCount": 27
            }
        ],
        "thoughtsTokenCount": 238,
        "serviceTier": "standard"
    },
    "modelVersion": "gemini-2.5-flash",
    "responseId": "bE0zatGsOJXOvr0P84OwqQY"
}
```

## 參考資料
systemInstruction 變成了你的「知識庫託管所」
我們把「規則（必須根據參考資料回答）」和「真正的參考資料內容（減肥就是少吃多動...）」通通塞進 systemInstruction 的 text 裡。這等於是在 Gemini 的大腦深處刻下這條家規。

```
curl "https://generativelanguage.googleapis.com/v1beta/models/gemini-2.5-flash:generateContent" \
  -H "x-goog-api-key: 你的_API_KEY" \
  -H "Content-Type: application/json" \
  -X POST \
  -d '{
    "systemInstruction": {
      "parts": [
        {
          "text": "【重要任務】\n你必須完全根據下方提供的『參考資料』來回答使用者的問題。禁止說你找不到，請直接把參考資料的內容整理出來！\n\n【參考資料】\n1. 減肥就是少吃多動\n2. 跑步是很好的運動\n3. 節制食量，控制飲食\n4. 早睡早起"
        }
      ]
    },
    "contents": [
      {
        "role": "user",
        "parts": [
          {
            "text": "如何減肥"
          }
        ]
      }
    ]
  }'
```
回覆內容:
```
{
    "candidates": [
        {
            "content": {
                "parts": [
                    {
                        "text": "根據參考資料，減肥的方法如下：\n\n1.  **少吃多動**：減肥的核心原則是減少攝取、增加消耗。\n2.  **節制食量，控制飲食**：透過管理飲食來減少熱量攝取。\n3.  **跑步**：這是一種很好的運動方式，有助於增加活動量。\n4.  **早睡早起**：保持良好的作息習慣。"
                    }
                ],
                "role": "model"
            },
            "finishReason": "STOP",
            "index": 0
        }
    ],
    "usageMetadata": {
        "promptTokenCount": 82,
        "candidatesTokenCount": 94,
        "totalTokenCount": 497,
        "promptTokensDetails": [
            {
                "modality": "TEXT",
                "tokenCount": 82
            }
        ],
        "thoughtsTokenCount": 321,
        "serviceTier": "standard"
    },
    "modelVersion": "gemini-2.5-flash",
    "responseId": "PFQzapzcDJWcvr0P-ZCP4A0"
}
```

## 設定角色身份
systemInstruction 為角色設定。<br>
```
curl "https://generativelanguage.googleapis.com/v1beta/models/gemini-2.5-flash:generateContent" \
  -H "x-goog-api-key: 你的_API_KEY" \
  -H "Content-Type: application/json" \
  -X POST \
  -d '{
    "systemInstruction": {
      "parts": [
        {
          "text": "你是一個紫微斗數算命師。"
        }
      ]
    },
    "contents": [
      {
        "role": "user",
        "parts": [
          {
            "text": "你好，我在台灣出生，性別女，西元2000年6月1日，早上11點出生，我想知道我的命宮有什麼星?"
          }
        ]
      },
      {
        "role": "model",
        "parts": [
          {
            "text": "你的命宮為太陰"
          }
        ]
      },
      {
        "role": "user",
        "parts": [
          {
            "text": "那我的官祿宮呢？"
          }
        ]
      }
    ]
  }'
```