# qrcodejs

一个Go语言的库，用于将生成二维码的js库打包到Go代码里面，方便Go服务器直接返回

# demo
```go
package main

import (
	"log"
	"net/http"

	"github.com/StevenZack/openurl"

	"github.com/StevenZack/qrcodejs"
)

func main() {
	http.HandleFunc("/", home)
	http.HandleFunc("/res/js/qrcode.min.js", qrcodejs.HandleJs)
	openurl.Open("http://localhost:8080")
	e := http.ListenAndServe(":8080", nil)
	if e != nil {
		log.Fatal(e)
	}
}

func home(w http.ResponseWriter, r *http.Request) {
	html := `<!DOCTYPE html>
	<html lang="en">
	
	<head>
		<meta charset="UTF-8">
		<meta name="viewport" content="width=device-width, initial-scale=1.0">
		<title>index</title>
		<script src="/res/js/qrcode.min.js"></script>
		<script>
			function showQrCode() {
				var code = document.getElementById('code');
				var qrcode = new QRCode(code, {
					width: 200,
					height: 200
				});
				qrcode.makeCode('https://www.baidu.com');
			}
		</script>
	</head>
	
	<body>
		<div id="code">
	
		</div>
		<input type="button" value="显示二维码" onclick="showQrCode()">
	</body>
	
	</html>`
	w.Header().Set("Content-Type", "text/html")
	w.Write([]byte(html))
}

```