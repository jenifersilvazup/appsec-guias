<h1 style="font-weight:bold">
    <img src="./resources/react.png" width="40px" style="vertical-align: middle">
    Requisitos de Segurança React Native 
</h1>

## Conteúdo

- [Tratamento de entradas](#data_binding)
- [Vulnerabilidades em componentes](#vulnerable_components)
- [Armazenamento de dados sensíveis](#sensitive_data)
  
- - -

<h2 id="data_binding" style="font-weight:bold">
  <img src="./resources/privacy.svg" alt="Logo Armazenamento de Dados e Privacidade" width="40px" />
	Tratamento de entradas
</h2>

Ao trabalhar com aplicações web, uma das grandes preocupações que precisamos ter é a forma que lidamos com as entradas no sistema para evitar os ataques de injeção de código malicioso e cross site scripting (xss). Uma das principais remediações para estes ataques é sempre utilizar algoritimos de sanitização de entradas.

<table>
  <thead>
    <tr>
      <th></th>
      <th>Requisito</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td width="20px"><img src="./resources/check.svg" width="20px"/></td>
      <td><strong>Utilize</strong> a vinculação de dados com chaves {} quando estamos lidando com valores textContent</td>
    </tr>
    <tr>
      <td width="20px"><img src="./resources/check.svg" width="20px"/></td>	
      <td><strong>Evite</strong> injetar conteúdos diretamente no DOM. Caso seja necessário, o ideal é sanitizar os dados antes da atribuição com dompurify</td>
    </tr>
    <tr>
      <td width="20px"><img src="./resources/check.svg" width="20px"/></td>
      <td><strong>Utilize</strong> validações para garantir que os links são http: ou https: ao trabahar com URLs utilizando um parse nativo da linguagem.</td>
    </tr>
    <tr>
      <td width="20px"><img src="./resources/check.svg" width="20px"/></td>
      <td><strong>Sempre</strong> escape os caracteres < ao enviar dados JSON junto de páginas React renderizadas pelos servidores</td>
    </tr>
    <tr>
      <td width="20px"><img src="./resources/check.svg" width="20px"/></td>
      <td><strong>Nunca</strong> concatene dados com o output de renderToStaticMarkup() para evitar ataques de XSS</td>
    </tr>
    <tr>
      <td width="20px"><img src="./resources/check.svg" width="20px"/></td>
      <td><strong>Evite</strong> utilizar bibliotecas que utilizam dangerouslySetInnerHTML, innerHTML URLs invalidadas ou templates inseguros</td>
    </tr>
  </tbody>
<table>

- - -

<h3 style="font-weight:bold">Exemplo</h3>

* Utilizando chaves {} para realizar data binding:

<pre>
	// Implementação correta:
	<div>{data}</div>

	// Implementação incorreta:
	<form action={data}>...
</pre>

* Exemplo de utilização do dompurify para sanitizar código:
<pre>
	// Implementação correta
	import purify from "dompurify";
	<div dangerouslySetInnerHTML={{__html:purify.sanitize(data) }} />

	// Implementação incorreta
	this.myRef.current.innerHTML = attackerControlledValue;
</pre>

* Exemplo de utilização do dompurify para sanitizar código html antes :
<pre>
	// Implementação correta
	import purify from "dompurify";
	<div dangerouslySetInnerHTML={{__html:purify.sanitize(data) }} />

	// Implementação incorreta
	this.myRef.current.innerHTML = attackerControlledValue;
</pre>

* Escape de caracteres <:
<pre>
	// Exemplo de implementação com escape:
	window.__PRELOADED_STATE__ =   ${JSON.stringify(preloadedState).replace( &lt/g, '\\u003c')};
</pre>
<h3 style="font-weight:bold">Guias e Artigos</h3>

* <i>[Sistema Android Keystore](https://developer.android.com/training/articles/keystore?hl=pt-br)
* [Dicas de Segurança Android](https://developer.android.com/training/articles/security-tips?hl=pt-br)</i>
  
- - -

<h2 id="vulnerable_components" style="font-weight:bold">
  <img src="./resources/crypt.svg" alt="Logo Criptografia" width="40px" />
  Vulnerabilidades em componentes
</h2>

É muito comum utilizarmos componentes de terceiros para auxiliar no desenvolvimento de código, principalmente quando algo que precisamos já pode ser encontrado pronto. Entretanto, sempe devemos nos atentar aos 

<table>
  <thead>
    <tr>
      <th></th>
      <th>Requisitos</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td width="20px"><img src="./resources/check.svg" width="20px"/></td>
      <td><strong>Utilize</strong> sempre as versões do react e do react-dom mais atualizadas</td>
    </tr>
    <tr>
      <td width="20px"><img src="./resources/check.svg" width="20px"/></td>
      <td><strong>Utilize</strong> ferramentas de análise estatica de código em todo o processo de desenvolvimento</td>
    </tr>
    <tr>
      <td width="20px"><img src="./resources/check.svg" width="20px"/></td>
      <td><strong>Utilize</strong> ferramentas de scan de dependências e se atente aos alertas para garantir que não esteja utlizando componentes vulneráveis</td>
    </tr>
    <tr>
      <td width="20px"><img src="./resources/check.svg" width="20px"/></td>
      <td><strong>Execute</strong> revisões manuais e com ferramentas de scan de código em bibliotecas que pretendem utilizar</td>
    </tr>
  </tbody>
<table>

- - -

<h2 id="sensitive_data" style="font-weight:bold">
  <img src="./resources/auth.svg" alt="Logo Autenticação e Gerenciamento de Sessão" width="40px" />
  Armazenamento de dados sensíveis
</h2>

<table>
  <thead>
    <tr>
      <th></th>
      <th>Requisito</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td width="20px"><img src="./resources/check.svg" width="20px"/></td>
      <td><strong>Nunca</strong>armazene chaves de APIs sensíveis em seu código, opte sempre por utilizar ferramentas como <i>react-native-dotenv</i> ou <i>react-native-config</i></td>
    </tr>
    <tr>
      <td width="20px"><img src="./resources/check.svg" width="20px"/></td>
      <td><strong>A forma correta</strong> de lidar com a necessidade de possuir informações sensíeis na aplicação é construir uma camada de orquestração entre seu aplicativo e o recurso</td>
    </tr>
    <tr>
      <td width="20px"><img src="./resources/check.svg" width="20px"/></td>
      <td><strong>Se a autenticação baseada em <i>token</i> sem estado for usada (<i>stateless token-based authentication</i>)</strong>, o servidor <strong>deverá</strong> fornecer um <i>token</i> que foi assinado usando um algoritmo seguro;</td>
    </tr>
<table>

- - -

<h3 style="font-weight:bold">Guias e Artigos</h3>

- <i>[Documentação oficial de segurança do React Native](https://developer.android.com/studio/publish/preparing?hl=pt-br)
- [<i>React Native Security</i>](https://www.appsealing.com/react-native-security/)</i>
  
- - -

## Licenças

Ícones feitos por [Freepik](https://www.flaticon.com/authors/freepik), [monkik](https://www.flaticon.com/authors/monkik), [Flat Icons](https://www.flaticon.com/authors/flat-icons) e [Kiranshastry](https://www.flaticon.com/authors/kiranshastry) de [www.flaticon.com](https://www.flaticon.com/)







