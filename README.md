# ImageDiff

- ImageDiff(イメージ・デフ)はTIFFおよびPDFの画像データの差分を表示するアプリケーションです。
- ImageDiff is an application to display the differences of multiple image files of either TIFF or PDF.

[Sample PDF](./Sample/Output_Sample_001a.pdf)

## Remark

- This application works only in 64 bit Windows.

- This application includes [Poppler](https://github.com/oschwartz10612/poppler-windows). It is confirmed that [Poppler](https://github.com/oschwartz10612/poppler-windows) versions of 23.x or 24.x series are applicable, but 25.x series is not currently applicable to make this program work. The recommended version is [24.08.0](https://github.com/oschwartz10612/poppler-windows/releases/tag/v24.08.0-0).

## Test Steps

- The developers perform the test by the following steps.

1. Copy to the local repo.

      ```bash
   git clone https://github.com/Charlie-Aki/ImageDiff.git
   cd ./ImageDiff/
   ```

2. Make a virtual environment

   ```bash
   conda create -n ImageDiff python=3.11.0
   conda activate ImageDiff
   pip install -r requirements.txt
   ```

3. Code test

   ```bash
   python main.py
   ```

- The following line shall be changed in pdf2image.py so that the console blinking issue will be eliminated. (It looks that the official pdf2image developers recognize the issue, but they have not fully fixed it yet. We are waiting for the update.)

   ```python
   # Replace this line
   proc = Popen(command, env=env, stdout=PIPE, stderr=PIPE)
   # like the following:
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

## License

- ImageDiff is licensed under [GPLv3](./LICENSE) because the execution file is packed with [Poppler](https://github.com/oschwartz10612/poppler-windows).
