あに瓶さんの利用しているソフトが古く、メンテナンスもされていないようなので、再検討した上で独自の変換ルールを使用します。

- 必要なソフトウェア
  
  - ImageMagick
    
    - Version:>=6.5.4-3(MUST),>=7(SHOULD)
    
    - With FFT(convert -fftが利用可能であること)
    
    - Q16(MUST,Q8は不可) HDRI対応(内部的に32bit)
    
    - Linux/MacOS推奨。WSLでも可

実際の手順

INPUT(1920x1080)を処理したい画像,OUTPUTを最終的に使う画像とします。 

> convert INPUT -gravity North -background black -extent 1920x1920 -fft -delete 1 -contrast-stretch 0 -evaluate log 100000 -gamma 1.5 OUTPUT

このワンライナーであに瓶に準拠しつつ、より単純なFHD/HD/SDの判定に向いた(主観)画像が生成されます。

意味は順番に

-gravity North -background black -extent 1920x1920 : 上にそろえて1920x1920にキャンパスを拡大、背景は黒

-fft -delete 1 : DFTを実行。0は複素数の絶対値で1は位相(偏角)で、今使いたいのは絶対値のみなので偏角成分を破棄。

-contrast-stretch 0 -evaluate log 100000 -gamma 1.5 : DFTで得られた画像はそのままだと真っ黒なので、人間の感覚に合わせ対数スケールへ変換。同時にコントラストを最大化する。その後ガンマ値を+1.5に補正。

ソース画像が1920x1080でない場合、-resizeを用いて拡大縮小を行う。(拡大時は基本Sinc,縮小時はLanzcos)
