<h1 style="font-weight:bold">
    <img src="./resources/kotlin.svg" width="40px" style="vertical-align: middle">
    Requisitos de Segurança Kotlin 
</h1>

## Conteúdo

- [Armazenamento de Dados e Privacidade](#data_privacy)
- [Criptografia](#crypto)
- [Autenticação e Gerenciamento de Sessão](#auth)
- [Comunicação de Rede](#network)
- [Interação com a Plataforma](#platform_interaction)
- [Qualidade de Código e Configuração do Compilador](#code_quality)
  
- - -

<h2 id="data_privacy" style="font-weight:bold">
  <img src="./resources/privacy.svg" alt="Logo Armazenamento de Dados e Privacidade" width="40px" />
  Armazenamento de Dados e Privacidade
</h2>

A proteção de dados sensíveis, como credenciais de usuário e informações pessoais, é um foco chave na segurança de aplicações. Primeiramente, dados sensíveis podem ser expostos de forma não-intencional a outros aplicativos rodando no mesmo dispositivo se os mecanismos do sistema operacional como o <i>IPC</i> estiverem sendo usados de forma incorreta. Os dados também podem ser vazados não-intencionalmente no armazenamento em nuvem, <i>backups</i> ou no <i>cache</i> do teclado. Além disso, dispositivos móveis podem ser perdidos ou roubados de forma mais comum que outros tipos de dispositivos, então um atacante conseguindo acesso físico ao equipamento é um cenário mais possível. Nesse caso, proteções adicionais podem ser implementadas para tornar a recuperação de dados sensíveis mais difícil.

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
      <td><strong>Utilize</strong> recursos de armazenamento de credenciais do sistema como <a href="https://developer.android.com/training/articles/keystore">KeyStore</a> para armazenar dados sensíveis;</td>
    </tr>
    <tr>
      <td width="20px"><img src="./resources/check.svg" width="20px"/></td>
      <td><strong>Não armazene</strong> dados sensíveis fora do contêiner do aplicativo ou de recursos de armazenamento de credenciais do sistema;</td>
    </tr>
    <tr>
      <td width="20px"><img src="./resources/check.svg" width="20px"/></td>
      <td><strong>Remova</strong> dados sensíveis dos <i>logs</i> de aplicação;</td>
    </tr>
    <tr>
      <td width="20px"><img src="./resources/check.svg" width="20px"/></td>
      <td><strong>Não compartilhe</strong> dados sensíveis com terceiros, exceto se for uma parte necessária da arquitetura;</td>
    </tr>
    <tr>
      <td width="20px"><img src="./resources/check.svg" width="20px"/></td>
      <td><strong>Desabilite</strong> o <i>cache</i> do teclado nas entradas de usuário que processam dados sensíveis;</td>
    </tr>
    <tr>
      <td width="20px"><img src="./resources/check.svg" width="20px"/></td>
      <td><strong>Não exponha</strong> dados sensíveis, como senhas ou <i>PINs</i> através de mecanismos <i>IPC</i> ou interface gráfica;</td>
    </tr>
  </tbody>
<table>

- - -

<h3 style="font-weight:bold">Exemplo</h3>

* Desabilitando o <i>cache</i> do teclado em entrada de senhas:
<pre>
/* Utilizando inputType textPassword */
&ltEditText 
  android:inputType="textPassword"
/&gt
</pre>

* Exemplo de utilização do IPC incorreta:
<pre>
// compartilhando senha em texto plano
val sendIntent: Intent = Intent().apply {
  action = Intent.ACTION_SEND
  putExtra(Intent.EXTRA_TEXT, "password12345")
  type = "text/plain"
}

val shareIntent = Intent.createChooser(sendIntent, null)
startActivity(shareIntent)
</pre>

* Exemplo de <i>log</i> que não deve existir na sua aplicação:
<pre>
  fun xpto() {
    // regra de negócio
    Log.i("Info", "Número do Cartão do Usuário:" + numCartaoUsuario);
  }
</pre>

<h3 style="font-weight:bold">Guias e Artigos</h3>

* <i>[Sistema Android Keystore](https://developer.android.com/training/articles/keystore?hl=pt-br)
* [Dicas de Segurança Android](https://developer.android.com/training/articles/security-tips?hl=pt-br)</i>
  
- - -

<h2 id="crypto" style="font-weight:bold">
  <img src="./resources/crypt.svg" alt="Logo Criptografia" width="40px" />
  Criptografia
</h2>

A criptografia é um componente essencial quando se trata da proteção dos dados, principalmente quando armazenados em dispositivos móveis. É também uma categoria em que as coisas podem dar muito errado, especialmente quando as convenções e padrões não são seguidos.

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
      <td><strong>Não utilize</strong> criptografia simétrica com chaves fixas no código fonte como único método de criptografia;</td>
    </tr>
    <tr>
      <td width="20px"><img src="./resources/check.svg" width="20px"/></td>
      <td><strong>Utilize</strong> implementações comprovadas de primitivas criptográficas, como: <i>SHA-256</i>, <i>AES</i>, <i>RSA</i>, entre outras;</td>
    </tr>
    <tr>
      <td width="20px"><img src="./resources/check.svg" width="20px"/></td>
      <td><strong>Utilize</strong> primitivas criptográficas que são apropriadas para o caso de uso em questão, configurado com parâmetros que são aderentes às melhores práticas da indústria;</td>
    </tr>
    <tr>
      <td width="20px"><img src="./resources/check.svg" width="20px"/></td>
      <td><strong>Não utilize</strong> protocolos criptográficos ou algoritmos que são considerados amplamente obsoletos para uso em segurança, como: <i>SHA-1</i>, <i>MD5</i>, entre outros;</td>
    </tr>
    <tr>
      <td width="20px"><img src="./resources/check.svg" width="20px"/></td>
      <td><strong>Não utilize</strong> a mesma chave criptográfica para vários fins;</td>
    </tr>
    <tr>
      <td width="20px"><img src="./resources/check.svg" width="20px"/></td>
      <td><strong>Utilize</strong> um gerador de números aleatórios seguro, como o <code><a src="https://developer.android.com/reference/kotlin/java/security/SecureRandom">java.security.SecureRandom</a></code> para valores aleatórios;</td>
    </tr>
  </tbody>
<table>

- - - -

<h3 style="font-weight:bold">Exemplo</h3>

* Gerando número aleatório com SecureRandom:
<pre>
import java.security.SecureRandom

fun randomNumber() {
    val secureRandom = SecureRandom()
    secureRandom.nextInt(100)
}
</pre>

<h3 style="font-weight:bold">Guias e Artigos</h3>

- <i>[Criptografia Android](https://developer.android.com/guide/topics/security/cryptography?hl=pt-br)
- [Segurança e Persistência Android com a Biblioteca Hawk](https://www.thiengo.com.br/seguranca-e-persistencia-android-com-a-biblioteca-hawk)</i>
- - -

<h2 id="auth" style="font-weight:bold">
  <img src="./resources/auth.svg" alt="Logo Autenticação e Gerenciamento de Sessão" width="40px" />
  Autenticação e Gerenciamento de Sessão
</h2>

Na maioria dos casos, o <i>login</i> de usuários a um serviço remoto é parte integrante da arquitetura global das aplicações móveis. Embora a maior parte da lógica aconteça no terminal, alguns requisitos básicos sobre a forma como as contas e sessões dos usuários devem ser gerenciadas.

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
      <td><strong>Se o aplicativo fornecer aos usuários acesso a um serviço remoto</strong>, alguma forma de autenticação <strong>deverá</strong> ser executada;</td>
    </tr>
    <tr>
      <td width="20px"><img src="./resources/check.svg" width="20px"/></td>
      <td><strong>Se o gerenciamento de sessão com estado for usado (<i>stateful session management</i>)</strong>, o terminal remoto <strong>deverá</strong> usar identificadores de sessão gerados aleatoriamente para autenticar as solicitações de clientes, sem que as credenciais do usuário sejam enviadas;</td>
    </tr>
    <tr>
      <td width="20px"><img src="./resources/check.svg" width="20px"/></td>
      <td><strong>Se a autenticação baseada em <i>token</i> sem estado for usada (<i>stateless token-based authentication</i>)</strong>, o servidor <strong>deverá</strong> fornecer um <i>token</i> que foi assinado usando um algoritmo seguro;</td>
    </tr>
    <tr>
      <td width="20px"><img src="./resources/check.svg" width="20px"/></td>
      <td>O endpoint remoto <strong>deverá</strong> terminar a sessão existente quando o usuário efetuar <i>logout</i>;</td>
    </tr>
    <tr>
      <td width="20px"><img src="./resources/check.svg" width="20px"/></td>
      <td><strong>Tenha</strong> uma política de senha e exija-a no terminal remoto;</td>
    </tr>
    <tr>
      <td width="20px"><img src="./resources/check.svg" width="20px"/></td>
      <td>O terminal remoto <strong>deverá</strong> implementar um mecanismo para proteger contra o envio de credenciais em um número excessivo;</td>
    </tr>
    <tr>
      <td width="20px"><img src="./resources/check.svg" width="20px"/></td>
      <td><strong>Invalide</strong> sessões pelo terminal remoto após um período predefinido de inatividade e <strong>expire</strong> os <i>tokens</i> de acessos;</td>
    </tr>
    <tr>
      <td width="20px"><img src="./resources/check.svg" width="20px"/></td>
      <td><strong>Defina e aplique</strong> modelos de autorização no terminal remoto;</td>
    </tr>
  </tbody>
<table>

- - -

<h3 style="font-weight:bold">Guias e Tutoriais</h3>

- <i>[Autenticar para os Serviços OAuth2](https://developer.android.com/training/id-auth/authenticate?hl=pt_br)</i>

- - -

<h2 id="network" style="font-weight:bold">
  <img src="./resources/https.svg" alt="Logo Comunicação de Rede" width="40px" />
  Comunicação de Rede
</h2>

O propósito desses requisitos é garantir a confidencialidade e integridade das informações trocadas entre o aplicativo móvel e os terminais de serviço remoto. Um aplicativo móvel deve, pelo menos, criar um canal seguro e criptografado para comunicação de rede utilizando o protocolo <i>TLS</i> com as configurações apropriadas.

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
      <td><strong>Utilize</strong> <i>TLS</i> para criptografar os dados trafegados na rede;</td>
    </tr>
    <tr>
      <td width="20px"><img src="./resources/check.svg" width="20px"/></td>
      <td><strong>Configure</strong> o <i>TLS</i> de acordo com as melhores práticas atuais;</td>
    </tr>
    <tr>
      <td width="20px"><img src="./resources/check.svg" width="20px"/></td>
      <td><strong>Verifique</strong> o certificado X.509 do terminal remoto quando o canal seguro for estabelecido. Apenas certificados assinados por uma <i>CA</i> confiável deverão ser aceitos;</td>
    </tr>
  </tbody>
<table>

- - -

<h3 style="font-weight:bold">Exemplos</h3>

- Fazendo uma requisição utilizando <i>HTTPS</i>:
<pre>
val url = URL("https://www.zup.com.br/")
val urlConnection: URLConnection = url.openConnection()
val inputStream: InputStream = urlConnection.getInputStream()
copyInputStreamToOutputStream(inputStream, System.out)
</pre>

- Adicionando um certificado Zup como confiável:
<pre>
// Carrega as CAs de um InputStream
// (Pode ser de um resource ou ByteArrayInputStream)
val cf: CertificateFactory = CertificateFactory.getInstance("X.509")

val caInput: InputStream = BufferedInputStream(FileInputStream("ZUPCORPCA.crt"))
val ca: X509Certificate = caInput.use {
    cf.generateCertificate(it) as X509Certificate
}

// Cria uma KeyStore contendo nossas CAs de confiança
val keyStoreType = KeyStore.getDefaultType()
val keyStore = KeyStore.getInstance(keyStoreType).apply {
    load(null, null)
    setCertificateEntry("ca", ca)
}

// Cria um TrustManager que confia nas CAs inseridas no nosso KeyStore
val tmfAlgorithm: String = TrustManagerFactory.getDefaultAlgorithm()
val tmf: TrustManagerFactory = TrustManagerFactory.getInstance(tmfAlgorithm).apply {
    init(keyStore)
}

// cria um SSLContext que utiliza nosso TrustManager
val context: SSLContext = SSLContext.getInstance("TLS").apply {
    init(null, tmf.trustManagers, null)
}

// faz a URLConnection utilizar a SocketFactory do nosso SSLContext
val url = URL("https://manager.horusec.zup.corp/")
val urlConnection = url.openConnection() as HttpsURLConnection
urlConnection.sslSocketFactory = context.socketFactory
val inputStream: InputStream = urlConnection.inputStream
copyInputStreamToOutputStream(inputStream, System.out)
</pre>

<h3 style="font-weight:bold">Guias e Artigos</h3>

- <i>[Segurança com <i>HTTPS</i> e <i>SSL</i>](https://developer.android.com/training/articles/security-ssl?hl=pt-br)</i>

- - -

<h2 id="platform_interaction" style="font-weight:bold">
  <img src="./resources/android.svg" alt="Logo Interação com a Plataforma" width="40px" />
  Interação com a Plataforma
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
      <td><strong>Solicite</strong> o conjunto mínimo de permissões necessárias para execução do aplicativo;</td>
    </tr>
    <tr>
      <td width="20px"><img src="./resources/check.svg" width="20px"/></td>
      <td><strong>Valide</strong> as entradas de fontes externas e do usuário e, se necessário, <strong>sanitize-as</strong>. Isso inclui dados recebidos através da <i>UI</i>, mecanismos de <i>IPC</i> como <i>intents</i>, <i>URLs</i> personalizados e origens pela rede;</td>
    </tr>
    <tr>
      <td width="20px"><img src="./resources/check.svg" width="20px"/></td>
      <td><strong>Não exporte</strong> funcionalidades sensíveis através de esquemas de <i>URL</i> personalizado, a menos que esses mecanismos estejam devidamente protegidos;</td>
    </tr>
    <tr>
      <td width="20px"><img src="./resources/check.svg" width="20px"/></td>
      <td><strong>Não exporte</strong> funcionalidades sensíveis através do <i>IPC</i>, a menos que esses mecanismos estejam devidamente protegidos;</td>
    </tr>
    <tr>
      <td width="20px"><img src="./resources/check.svg" width="20px"/></td>
      <td><strong>Desative</strong> código JavaScript nos <i>WebViews</i>, a menos que explicitamente necessário;</td>
    </tr>
    <tr>
      <td width="20px"><img src="./resources/check.svg" width="20px"/></td>
      <td>Configure os <i>WebViews</i> para permitir apenas o conjunto mínimo de manipuladores de protocolo necessários (preferencialmente, apenas <i>HTTPS</i>). <strong>Desabilite</strong> manipuladores potencialmente perigosos, como um arquivo, tel e app-id estão;</td>
    </tr>
    <tr>
      <td width="20px"><img src="./resources/check.svg" width="20px"/></td>
      <td><strong>Se os métodos nativos do aplicativo forem expostos a um <i>WebView</i></strong>, verifique se o <i>WebView</i> só renderiza o código JavaScript contido no pacote do aplicativo;</td>
    </tr>
    <tr>
      <td width="20px"><img src="./resources/check.svg" width="20px"/></td>
      <td><strong>Se houver a desserialização de objetos</strong>, a mesma é implementada usando <i>APIs</i> de serialização seguras, como a oferecida pela própria JetBrains: <code><a href="https://kotlinlang.org/docs/reference/serialization.html">kotlinx-serialization</a></code>;</td>
    </tr>
  </tbody>
<table>

- - -

<h3 style="font-weight:bold">Guias e Artigos</h3>

- [<i>Serialization Guide</i>](https://github.com/Kotlin/kotlinx.serialization/blob/master/docs/serialization-guide.md)
- [<i>Enable Disable JavaScript On Webview in Android</i>](https://www.android-examples.com/enable-disable-javascript-on-webview-in-android/)
- [<i>How Can I Validate EditText Input in Android Using Kotlin?</i>](https://www.tutorialspoint.com/how-can-i-validate-edittext-input-in-android-using-kotlin)
- [<i>Easy EditText Contet Validation With Kotlin</i>](https://adrianhall.github.io/android/2018/04/11/easy-edittext-content-validation-with-kotlin/)

- - -

<h2 id="code_quality" style="font-weight:bold">
  <img src="./resources/code.svg" alt="Logo Qualidade de Código e Configuração do Compilador" width="40px" />
  Qualidade de Código e Configuração do Compilador
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
      <td><strong>Assine e disponibilize</strong> o aplicativo somente com um certificado válido e cuja chave privada está devidamente protegida;</td>
    </tr>
    <tr>
      <td width="20px"><img src="./resources/check.svg" width="20px"/></td>
      <td>Crie o aplicativo em modo de <i>release</i> e com as configurações apropriadas para ela (ex.: <i>non-debuggable</i>);</td>
    </tr>
    <tr>
      <td width="20px"><img src="./resources/check.svg" width="20px"/></td>
      <td><strong>Remova</strong> os símbolos de depuração dos binários nativos;</td>
    </tr>
    <tr>
      <td width="20px"><img src="./resources/check.svg" width="20px"/></td>
      <td><strong>Remova</strong> todo código de depuração e de assistência ao desenvolvedor (ex.: código de teste, <i>backdoors</i>, configurações ocultas). <strong>Remova</strong> também <i>logs</i> detalhados de erros e mensagens de depuração;</td>
    </tr>
    <tr>
      <td width="20px"><img src="./resources/check.svg" width="20px"/></td>
      <td><strong>Identifique e revise</strong> quanto à vulnerabilidades conhecidas todos componentes de terceiros utilizados pelo aplicativo, como bibliotecas e <i>frameworks</i>;</td>
    </tr>
    <tr>
      <td width="20px"><img src="./resources/check.svg" width="20px"/></td>
      <td><strong>Capture e trate</strong> devidamente as possíveis exceções;</td>
    </tr>
    <tr>
      <td width="20px"><img src="./resources/check.svg" width="20px"/></td>
      <td><strong>Bloqueie</strong> acesso por padrão no tratamento de erros com controles de segurança;</td>
    </tr>
    <tr>
      <td width="20px"><img src="./resources/check.svg" width="20px"/></td>
      <td><strong>Corrija</strong> problemas de vazamento de memória;</td>
    </tr>
    <tr>
      <td width="20px"><img src="./resources/check.svg" width="20px"/></td>
      <td><strong>Habilite</strong> funcionalidades de segurança gratuitas das ferramentas, tais como minificação do <i>byte-code</i>, proteção de pilha, suporte PIE e contagem automática de referência;</td>
    </tr>
  </tbody>
<table>

- - -

<h3 style="font-weight:bold">Guias e Artigos</h3>

- <i>[Preparar Aplicativo para o Lançamento](https://developer.android.com/studio/publish/preparing?hl=pt-br)
- [<i>9 Ways to Avoid Memory Leaks in Android</i>](https://android.jlelse.eu/9-ways-to-avoid-memory-leaks-in-android-b6d81648e35e)</i>
  
- - -

## Licenças

Ícones feitos por [Freepik](https://www.flaticon.com/authors/freepik), [monkik](https://www.flaticon.com/authors/monkik), [Flat Icons](https://www.flaticon.com/authors/flat-icons) e [Kiranshastry](https://www.flaticon.com/authors/kiranshastry) de [www.flaticon.com](https://www.flaticon.com/)
