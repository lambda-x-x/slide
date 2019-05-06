# 作成したスライドを置いておくプロジェクト（多分）
## スライドの作成方法
- `Pandoc` で `.md` -> `.pdf` にしている
- `pandoc ${INPUT_NAME}.md  -o {OUTPUT_NAME}.pdf --pdf-engine=lualatex -t beamer -V theme:Madrid -V colortheme:seahorse  -H h-luatexja.tex`
