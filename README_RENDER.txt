IPTV Player - pacote para Render

O index.html foi ajustado para usar:
<base href="/" />

Arquivos incluídos:
- index.html
- main.dart.js
- style.css
- flutter_service_worker.js
- manifest.json
- version.json
- favicon.png
- icons/Icon-192.png
- icons/Icon-512.png
- LICENSE

IMPORTANTE:
O build original referencia uma pasta assets/ com arquivos do Flutter
(AssetManifest.json, FontManifest.json, fontes, bandeiras etc.), mas essa pasta
não foi enviada na conversa. Portanto este ZIP contém tudo o que foi possível
montar com os arquivos recebidos, porém algumas telas/recursos podem falhar
até a pasta assets/ original ser adicionada.

No Render, publique como Static Site e use a raiz deste pacote como diretório
de publicação.
