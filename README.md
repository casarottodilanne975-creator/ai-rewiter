<!DOCTYPE html>
<html lang="zh-CN">
<head>
  <meta charset="UTF-8">
  <title>免费AI改写工具</title>
  <style>
    *{box-sizing:border-box;margin:0;padding:0;font-family:Arial,sans-serif}
    body{max-width:800px;margin:40px auto;padding:20px;background:#f8f9fa}
    .box{background:white;padding:30px;border-radius:16px;box-shadow:0 4px 20px rgba(0,0,0,0.1)}
    textarea{width:100%;height:200px;padding:16px;margin:15px 0;border:1px solid #ddd;border-radius:12px;font-size:16px}
    button{background:#0066ff;color:white;border:none;padding:14px 32px;border-radius:12px;cursor:pointer;font-size:16px;margin:10px 0}
    button:disabled{background:#999}
    .result{margin-top:20px;padding:20px;background:#f1f3f5;border-radius:12px;min-height:120px;white-space:pre-wrap}
  </style>
</head>
<body>
  <div class="box">
    <h2>🚀 免费AI文本改写工具</h2>
    <p style="color:#666">每天免费3次使用</p >
    <textarea id="input" placeholder="把你要改写的文章、句子粘贴在这里..."></textarea>
    <button onclick="rewrite()">✨ 一键改写</button>
    <div class="result" id="result"></div>
  </div>

  <div style="text-align:center;margin-top:30px;color:#555;font-size:15px">
    用不完？想无限使用？联系我：<br>
    <a href=" " target="_blank" style="color:#0066ff;font-weight:bold">@你的TG用户名</a >
  </div>

  <script>
    let count = 0;
    async function rewrite() {
      if(count >= 3) {
        document.getElementById("result").innerHTML = "免费次数用完啦！<br>联系 Telegram 获取无限使用权限 → @你的TG用户名";
        return;
      }
      
      const text = document.getElementById("input").value.trim();
      if(!text) return alert("请输入内容！");
      
      const btn = document.querySelector("button");
      btn.disabled = true;
      document.getElementById("result").innerText = "正在用AI改写中...请稍等";

      try {
        const res = await fetch("https://api.deepseek.com/v1/chat/completions", {
          method: "POST",
          headers: {
            "Content-Type": "application/json",
            "Authorization": "Bearer sk-XXXXXXXXXXXXXXXX"   // ← 这里后面要改成你的Key
          },
          body: JSON.stringify({
            model: "deepseek-chat",
            messages: [{role: "user", content: `请用更自然流畅的语言改写以下文字，保持原意：\n\n${text}`}]
          })
        });

        const data = await res.json();
        const result = data.choices?.[0]?.message?.content || "出错了，请重试";
        document.getElementById("result").innerText = result;
        count++;
      } catch(e) {
        document.getElementById("result").innerText = "请求失败，可能需要联系我获取新的API";
      }
      btn.disabled = false;
    }
  </script>
</body>
</html>
