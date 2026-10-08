import express from "express";
import cors from "cors";

const app = express();
app.use(cors());
app.use(express.json({ limit: "1mb" }));

const PORT = process.env.PORT || 3000;
const API_KEY = process.env.ANTHROPIC_API_KEY;
const MODEL = "claude-sonnet-4-6";

if (!API_KEY) {
  console.warn("ВНИМАНИЕ: переменная окружения ANTHROPIC_API_KEY не задана.");
}

function buildPrompt(text) {
  return (
    "Ты эксперт по арабской грамматике, который объясняет материал ученику среднего уровня — просто, кратко, без лишнего. Дан арабский текст без огласовок (харакят).\n\n" +
    'Текст: "' + text + '"\n\n' +
    "Выполни:\n" +
    "1. Расставь грамматически корректные огласовки на весь текст.\n" +
    "2. Дай перевод ВСЕГО текста целиком на русский и на узбекский — ПО СМЫСЛУ, естественной фразой, как сказал бы носитель языка, а НЕ дословно слово-в-слово.\n" +
    "3. Дай итоговый грамматический разбор (إعراب) всего предложения на арабском — МАКСИМУМ 1 короткое предложение, только самое важное (тип предложения, подлежащее и сказуемое). Без второстепенных деталей.\n" +
    "4. Раздели текст на отдельные слова по порядку (без знаков препинания как отдельных элементов).\n" +
    "5. Для каждого слова укажи только это, и ничего лишнего:\n" +
    '   - "surface": слово с огласовками\n' +
    '   - "translation_ru": краткий перевод на русский (1-3 слова)\n' +
    '   - "translation_uz": краткий перевод на узбекский латиницей (1-3 слова)\n' +
    '   - "grammar_ru": КОРОТКАЯ метка по-русски, 2-5 слов, НЕ предложение (например: "подлежащее, именительный падеж" или "глагол прошедшего времени")\n' +
    '   - "grammar_uz": та же метка на узбекском (латиница), тоже 2-5 слов\n' +
    '   - "irab": таркип (إعراب) ЭТОГО слова на арабском — МАКСИМАЛЬНО КОРОТКО, 2-4 слова, только суть (например: "فاعل مرفوع" или "فعل ماضٍ"), без длинных пояснений\n' +
    '   - "plural": форма множественного числа с огласовками, если это существительное, иначе null\n' +
    '   - "verb_forms": если это глагол — объект {"past","present","imperative","masdar"} с огласовками на арабском, иначе null\n\n' +
    "Будь предельно краток везде, где это возможно без потери смысла — ученик должен понять с первого взгляда, без лишнего текста. Это также ускоряет ответ.\n\n" +
    "Ответь ТОЛЬКО валидным JSON без пояснений и без markdown-разметки, в формате:\n" +
    '{"translation_ru": "...", "translation_uz": "...", "irab_summary": "...", "words": [{"surface": "...", "translation_ru": "...", "translation_uz": "...", "grammar_ru": "...", "grammar_uz": "...", "irab": "...", "plural": null, "verb_forms": null}]}'
  );
}

function buildDictPrompt(word) {
  return (
    "Ты арабско-русский/узбекский словарь для ученика среднего уровня. Дано слово на русском или узбекском языке.\n\n" +
    'Слово: "' + word + '"\n\n' +
    "Дай его арабский эквивалент — самое употребимое слово. Ответь ТОЛЬКО валидным JSON без пояснений и markdown, в формате:\n" +
    '{"surface": "слово на арабском с огласовками", "translation_ru": "краткий перевод на русский", "translation_uz": "краткий перевод на узбекский латиницей", "grammar_ru": "короткая метка 2-5 слов по-русски (например: существительное, мужской род)", "grammar_uz": "та же метка на узбекском", "plural": "форма мн. числа с огласовками, если существительное, иначе null", "verb_forms": {"past":"...","present":"...","imperative":"...","masdar":"..."} с огласовками, если это глагол, иначе null}\n\n' +
    "Будь предельно краток. Если слово не удаётся однозначно перевести, выбери самый вероятный и распространённый вариант."
  );
}

async function callClaude(prompt, maxTokens) {
  const response = await fetch("https://api.anthropic.com/v1/messages", {
    method: "POST",
    headers: {
      "Content-Type": "application/json",
      "x-api-key": API_KEY,
      "anthropic-version": "2023-06-01",
    },
    body: JSON.stringify({
      model: MODEL,
      max_tokens: maxTokens,
      messages: [{ role: "user", content: prompt }],
    }),
  });

  if (!response.ok) {
    const errText = await response.text();
    console.error("Anthropic API error:", response.status, errText);
    const err = new Error("upstream_error");
    err.code = "upstream_error";
    throw err;
  }

  const data = await response.json();
  const textBlock = (data.content || []).find((b) => b.type === "text");
  if (!textBlock) {
    const err = new Error("no_text_in_response");
    err.code = "no_text_in_response";
    throw err;
  }

  let cleaned = textBlock.text.trim();
  cleaned = cleaned.replace(/^```json\s*/i, "").replace(/```$/, "").trim();

  try {
    return JSON.parse(cleaned);
  } catch (e) {
    console.error("Не удалось распарсить JSON от модели:", cleaned);
    const err = new Error("invalid_json_from_model");
    err.code = "invalid_json_from_model";
    throw err;
  }
}

app.post("/api/analyze", async (req, res) => {
  const text = ((req.body && req.body.text) || "").trim();

  if (!text) return res.status(400).json({ error: "empty_text" });
  if (!API_KEY) return res.status(500).json({ error: "server_not_configured" });
  if (text.length > 2000) return res.status(400).json({ error: "text_too_long" });

  try {
    const parsed = await callClaude(buildPrompt(text), 4000);
    if (!Array.isArray(parsed.words)) {
      return res.status(502).json({ error: "malformed_response" });
    }
    res.json(parsed);
  } catch (e) {
    console.error(e);
    res.status(502).json({ error: e.code || "server_error" });
  }
});

app.post("/api/dictionary", async (req, res) => {
  const word = ((req.body && req.body.word) || "").trim();

  if (!word) return res.status(400).json({ error: "empty_word" });
  if (!API_KEY) return res.status(500).json({ error: "server_not_configured" });
  if (word.length > 100) return res.status(400).json({ error: "word_too_long" });

  try {
    const parsed = await callClaude(buildDictPrompt(word), 1000);
    if (!parsed.surface) {
      return res.status(502).json({ error: "malformed_response" });
    }
    res.json(parsed);
  } catch (e) {
    console.error(e);
    res.status(502).json({ error: e.code || "server_error" });
  }
});

app.get("/health", (req, res) => res.json({ ok: true }));

app.listen(PORT, () => {
  console.log(`Сервер Harakat запущен на порту ${PORT}`);
});
