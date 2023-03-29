# openai

Api key



### 谭皓天

```shell
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
```

### THH-Gmail

```shell
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

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
```





```shell
curl https://api.openai.com/v1/chat/completions \
  -H "Content-Type: application/json" \
  -H "Authorization: Bearer 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" \
  -d '{
     "model": "gpt-3.5-turbo",
     "messages": [{"role": "user", "content": "Say this is a test!"}],
     "temperature": 0.7
   }'



curl http://usa.imjcker.com:9000/v1/chat/completions \
  -H "Content-Type: application/json" \
  -H "Authorization: Bearer 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" \
  -d '{
    "model": "gpt-3.5-turbo",
    "messages": [{"role": "user", "content": "Hello!"}]
  }'

curl https://api.openai.com/v1/chat/completions \
  -H "Content-Type: application/json" \
  -H "Authorization: Bearer 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" \
  -d '{
    "model": "gpt-3.5-turbo",
    "messages": [
    {"role":"assistant","content":"根据2020年联合国统计数据，人口最多的国家是中国，约为14.56亿人。其次是印度，约为13.4亿人。"},
    {"role":"user","content":"这两个国家分别有多少个名族呢？"}
    ]
  }'
```

