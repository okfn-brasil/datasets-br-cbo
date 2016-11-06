# dataset-cbo

A Classificação Brasileira de Ocupações (**CBO**), instituída pela [portaria ministerial nº. 397 de 2002](http://www.mtecbo.gov.br/cbosite/pages/legislacao.jsf), tem por finalidade a identificação das ocupações no mercado de trabalho, para fins classificatórios junto aos registros administrativos e domiciliares.

Ela se encontra disponível apenas em formato PDF, atualmente (2016) em www.mtecbo.gov.br/cbosite/pages/download?tipoDownload=1

O objetivo deste projeto é traduzir o PDF para um banco de dados aberto padronizado, que a princípio corresponde a um arquivo simples de planilha fortato CSV com três colunas, código, termo associado ao código e status do termo ("sinônimo", "classificação" ou "ocupação").

O resultado deste projeto poderá ser oferecido como *dataset* dentro do "ecosistema" [Data Packaged Core Datasets](http://data.okfn.org/roadmap/core-datasets) brasileiro. A proposta de *local datasets* foi lançada nesta discussão [_Country-specific data package register_](https://discuss.okfn.org/t/3178), e esta seria uma das primeiras contribuições dentro da proposta.

# Procedimento
Todos os mataterais-fonte estão sendo mantidos na pasta `dumps`.
```sh
cd dumps
wget -c --output-document=CBO2002_Liv1.pdf www.mtecbo.gov.br/cbosite/pages/download?tipoDownload=1
wget -c http://portalfat.mte.gov.br/wp-content/uploads/2016/02/CBO2002_LISTA.pdf
pdftotext -layout CBO2002_LISTA.pdf
echo -e "codigo,termo,tipo" | cat CBO2002_LISTA.txt | grep -E "^[0-9]{2,}" | tr -s ',' ';' | sed  -r 's:     +:,:g' > ../data/lista.txt

```

O primeiro `wget` é apenas para a preservação do inteiro teor das definições CBO, que são fornecidas apenas em PDF e portanto difíceis de serem extraídas na forma de lista. O nome de arquivo é o mesmo que o originalmente obtido por link via navegador. 

O segundo `wget` obtém uma listagem oficial (mte.gov.br) da lista desejada, de onde supomos haver o conteúdo desejado &ndash; resta auditorar com outrosrecursos a lista de códigos apresentados no inteiro teor. 
A opção `-layout` foi testada depois da default e  `-raw`  por se mostrar mais adequada, recuperando linha a linha já num formato próximo ao de planilha. O `grep` garante a exclusão de cabeçalhos e rodapés, o `tr` evita eventuis ambiguidades com o separador CSV, e o `seg` faz o trabalho principal de conversão de formato, garantindo por fim uma lista estruturada.





