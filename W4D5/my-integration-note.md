# Integration note: AIDC-Online-Team 3 (v1, go-live)

- **base\_url**:
  `https://t09.aidc.nadir.sh/v1`

- **service root**:
  `https://t09.aidc.nadir.sh`

- **model id:** `Qwen/Qwen2.5-1.5B-Instruct-AWQ`

- **auth:** bearer key, handed over separately by DM to the consumer on-call; never stored in this file

- **modalities:** text in, text out, tool calls per the OpenAI schema

- **example call:**
  `curl -s https://t09.aidc.nadir.sh/v1/chat/completions -H "Authorization: Bearer $KEY" -H "Content-Type: application/json" -d '{"model":"Qwen/Qwen2.5-1.5B-Instruct-AWQ","messages":[{"role":"user","content":"hello from outside"}]}'`

- **SLOs we publish:** availability monitored during the go-live window; TTFT p95 and error-rate targets are not published because no retained tier-1 benchmark evidence is available

- **limits, declared honestly:** model context length 4096 tokens; concurrency knee was not measured in retained week-3 evidence

- **on-call:** Lama · team channel · response within 15 minutes during the go-live window
