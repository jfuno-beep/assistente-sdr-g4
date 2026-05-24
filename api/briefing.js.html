export default async function handler(req, res) {
  if (req.method !== "POST") return res.status(405).json({ error: "Método não permitido" });

  if (process.env.ACCESS_CODE && req.headers["x-access-code"] !== process.env.ACCESS_CODE) {
    return res.status(401).json({ error: "Código de acesso inválido" });
  }

  const { segmento, kind } = req.body || {};
  if (!segmento) return res.status(400).json({ error: "segmento ausente" });

  const prompts = {
    mercado: `Analista de mercado para PMEs no Brasil. Segmento: "${segmento}".
Responda APENAS com JSON válido e COMPLETO (sem markdown). Máx 22 palavras por item:
{"dores_principais":["4 dores específicas do dono/gestor"],"mercado":{"resumo":"1 frase sobre dinâmica no Brasil","numeros":"1 número/% concreto"},"noticias":[{"titulo":"movimento relevante recente","contexto":"por que importa"}],"tendencias":["3 tendências para 2026/2027"]}
3 itens em noticias.`,
    playbook: `Head de vendas consultivas da G4 Educação (escola de negócios para PMEs). Lead do segmento: "${segmento}".
Responda APENAS com JSON válido e COMPLETO (sem markdown). Máx 24 palavras por item:
{"perfil_decisor":"perfil do dono/gestor: mentalidade, medos, o que o move","gatilhos":["3 pontes entre uma dor e uma solução da G4, frase pronta pro SDR"],"perguntas_spin":["3 perguntas de descoberta que expõem a dor"],"objecoes":[{"objecao":"objeção típica","resposta":"contorno consultivo"}],"vocabulario":["6 termos do setor pro SDR soar como insider"]}
3 itens em objecoes.`,
  };

  const prompt = prompts[kind];
  if (!prompt) return res.status(400).json({ error: "kind inválido" });

  try {
    const r = await fetch("https://api.anthropic.com/v1/messages", {
      method: "POST",
      headers: {
        "Content-Type": "application/json",
        "x-api-key": process.env.ANTHROPIC_API_KEY,
        "anthropic-version": "2023-06-01",
      },
      body: JSON.stringify({
        model: process.env.MODEL || "claude-sonnet-4-6",
        max_tokens: 1500,
        messages: [{ role: "user", content: prompt }],
      }),
    });
    const data = await r.json();
    if (data.error) return res.status(502).json({ error: "API: " + (data.error.message || "erro") });

    const text = (data.content || []).map((i) => (i.type === "text" ? i.text : "")).filter(Boolean).join("\n");
    const clean = text.replace(/```json|```/g, "").trim();
    const m = clean.match(/\{[\s\S]*\}/);
    return res.status(200).json(JSON.parse(m ? m[0] : clean));
  } catch (e) {
    return res.status(500).json({ error: "Falha ao gerar: " + e.message });
  }
}
