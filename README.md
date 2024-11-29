# webscraping-no-Gazetadigital-com-python
webscraping no Gazeta digital com python

## Importando bibliotecas
    import requests
    from bs4 import BeautifulSoup

## Definindo Parâmetros
    url = "https://www.gazetadigital.com.br/"
    headers = {"User-Agent": "Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/131.0.0.0 Safari/537.36"}

## Fazendo a requisição e mostrando o resultado da busca
    requisicao = requests.get(url, headers=headers)
    print(requisicao)

## Mostrando as notícias

    def extrair_noticias(url):
      try:
        # Envia um GET para a página
        resposta = requests.get(url)
        resposta.raise_for_status()  # Garante que a resposta foi bem-sucedida
        conteudo_html = resposta.text

        # Faz o parsing do HTML com BeautifulSoup
        soup = BeautifulSoup(conteudo_html, 'html.parser')

        # Procura pelos links das notícias
        noticias = []
        for link in soup.find_all('a', href=True):
            titulo = link.get_text(strip=True)
            href = link['href']
            # Filtra apenas links relevantes (opcional: verifique a estrutura do site)
            if titulo and href.startswith('/editorias'):
                noticias.append({
                    "titulo": titulo,
                    "link": f"https://www.gazetadigital.com.br{href}"
                })

        return noticias

      except Exception as e:
        print(f"Erro ao processar a página: {e}")
        return []

    # Executa o programa
    noticias_extraidas = extrair_noticias(url)

    # Exibe os resultados
    if noticias_extraidas:
      for noticia in noticias_extraidas:
        print(f"Título: {noticia['titulo']}")
        print(f"Link: {noticia['link']}\n")
    else:
      print("Nenhuma notícia encontrada ou erro ao acessar a página.")
