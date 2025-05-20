![image](https://github.com/user-attachments/assets/0050fca5-ec9b-43c9-989f-c12ac2195871)

# football_n8n
AI Sports Match Prediction Workflow
## OpenAI Node Documentation

### Node: **OpenAI Chat Model (`gpt-4o`)**

**Model**: `gpt-4o`  
**Temperature**: `0.7`  
**Max Tokens**: `300`

**Reasoning**:
- **gpt-4o** is selected for its high accuracy in structured outputs and language translation (important for English and Ukrainian predictions).
- **Temperature 0.7** strikes a balance between creative output and consistency, helping generate realistic and slightly varied football match predictions without hallucinations.
- **Max tokens 300** is sufficient to return concise 1–2 sentence predictions in two languages, along with metadata (teams, date, pass flag).

---

## AI Agent Node Instructions

### Node: **AI Agent (LangChain Agent)**

**Prompt Configuration**:
- Uses a system message that defines a strict output structure:
```json
{
  "Teams": "[normalized names or 'no data']",
  "Match Date": "[YYYY-MM-DD or 'no data']",
  "Prediction EN": "[1-2 sentence forecast in English]",
  "Prediction UA": "[Same forecast in Ukrainian]",
  "Pass": true/false
}
```
- Includes rules for normalization of team names, strict date formatting, and ensures bilingual prediction output.
- **EN prediction rule**: “Team X likely to win X–X because [reason]”
- **UA prediction**: Direct, proper translation of the EN prediction
- `Pass = true` is allowed only if teams, date, and one of the prediction fields are valid.

---

## Suggestions for Extension or Hardening

1. **Add retry logic** for AI calls or implement fallback models if the LLM fails or exceeds rate limits.
2. **Include logging** (using n8n’s `Set` + `IF` or external webhook) for entries with `Pass = false` to audit edge cases or misclassified data.
3. **Improve validation** with stricter regex filters before submitting to OpenAI (e.g., to catch malformed dates or text blocks).
4. **Extend JSON schema** to optionally extract or generate match scores, venues, or betting odds predictions.
5. **Rate limit handling**: Add delay or loop throttling to support larger Airtable record volumes without hitting OpenAI/Airtable limits.
6. **Translation fallback**: Use a dedicated translator model for Ukrainian in case GPT output is poor or not contextually adapted.
