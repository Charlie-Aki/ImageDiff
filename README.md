# ImageDiff

ImageDiff(イメージ・デフ)はTIFFおよびPDFの画像データの差分を表示するアプリケーションです。
ImageDiff is an application to display the differences of multiple image files of either TIFF or PDF.

## 注意点/Remark

このアプリケーションはWindowsでしか使用できません。
This application works only in 64 bit Windows.

[Poppler](https://github.com/oschwartz10612/poppler-windows)を内包しています。対応する[Poppler](https://github.com/oschwartz10612/poppler-windows)のバージョンは23.x系、24.x系であれば動作することを確認しています。25.x系は非対応です。推奨は[24.08.0](https://github.com/oschwartz10612/poppler-windows/releases/tag/v24.08.0-0)です。
This application includes [Poppler](https://github.com/oschwartz10612/poppler-windows). It is confirmed that [Poppler](https://github.com/oschwartz10612/poppler-windows) versions of 23.x or 24.x series are applicable, but 25.x series is not currently applicable to make this program work. The recommended version is [24.08.0](https://github.com/oschwartz10612/poppler-windows/releases/tag/v24.08.0-0).

## テスト方法/Test Steps

開発者の環境では以下の手順でテストしています。
The developers perform the test by the following steps.

1. ローカルレポジトリへのコピー/Copy to the local repo.

      ```bash
   git clone https://github.com/Charlie-Aki/ImageDiff.git
   cd ./ImageDiff/
   ```

2. 仮想環境の作成/Make a virtual environment

   ```bash
   conda create -n ImageDiff python=3.11.0
   conda activate ImageDiff
   pip install -r requirements.txt
   ```

3. コードのテスト/Code test

   ```bash
   python main.py
   ```

なお、pdf2image.pyの中身を以下のように修正しておくとアプリ実行時にコンソール画面がチカチカ表示されるのを防ぐことができます。(公式も認識しているようですが、修正が未だ不十分な様子なので待機中。)
The following lines shall be changed in pdf2image.py so that the console error will be eliminated. (It looks that the official pdf2image developers recognize the issue, but they have not fully fixed it yet. We are waiting for the update.)

   ```python
   # 以下のラインを/Replace
   proc = Popen(command, env=env, stdout=PIPE, stderr=PIPE)
   # 以下にように差し替える/with the followings.
   ```

   ```diff
   - proc = Popen(command, env=env, stdout=PIPE, stderr=PIPE)
   + startupinfo = None
   + if platform.system() == "Windows":
   +    # this startupinfo structure prevents a console window from popping up on Windows
   +    startupinfo = subprocess.STARTUPINFO()
   +    startupinfo.dwFlags |= subprocess.STARTF_USESHOWWINDOW
   + proc = Popen(command, env=env, stdout=PIPE, stderr=PIPE, startupinfo=startupinfo)
   ```

## ライセンス/License

公開しているexeファイルは[Poppler](https://github.com/oschwartz10612/poppler-windows)を含めた状態で配布しているので[GPLv3](./LICENSE)を採用しています。
ImageDiff is licensed under [GPLv3](./LICENSE) because the execution file is packed with [Poppler](https://github.com/oschwartz10612/poppler-windows).
