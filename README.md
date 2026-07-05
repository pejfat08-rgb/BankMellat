# BankMellat
library Bank Mellat IRAN

# Load library
<pre>
$this->load->library('Mellat');
</pre>

# Send parameter to library and Send to Bank
<pre>
$this->Mellat->set_options($terminal,$username,$password,$amount,$order,$callback);
$this->Mellat->send();
</pre>

# Callback
<pre>
$this->Mellat->set_options($terminal,$username,$password);

$this->Mellat->get($_POST);
</pre>۰۹
req, res) => {
  const amount = Number(req.body.amount);
  const orderId = req.body.orderId ?? crypto.randomUUID();

  const returnUrl = "https://example.com/verify";
  const callbackUrl = "https://example.com/callback";

  const payload = {
    merchantId: MERCHANT_ID,
    orderId,
    amount,
    returnUrl,
    callbackUrl,
  };

  // تولید Sign
  payload.sign = makeSign(payload); // نام فیلد sign را مطابق ملت بگذارید: sign / SIGN / Signature

  // ارسال به درگاه با POST (فرم auto-submit)
  const formInputs = Object.entries(payload)
    .map(([k, v]) => `<input type="hidden" name="${k}" value="${String(v)}" />`)
    .join("");

  res.send(`
    <html lang="fa" dir="rtl">
      <body>
        <form id="f" action="${GATEWAY_URL}" method="post">
          ${formInputs}
        </form>
        <script>document.getElementById('f').submit();</script>
      </body>
    </html>
  `);
});

// Verify/callback را بعد از اینکه ساختار پاسخ ملت را دارید دقیق می‌کنم
app.post("/verify", (req, res) => {
  // const { status, orderId, ... } = req.body;
  res.send("ok");
});

app.listen(3000, () => console.log("Server running on :3000"));

فقط یک چیز لازم req.body.orderIdhttps://example.com/verify