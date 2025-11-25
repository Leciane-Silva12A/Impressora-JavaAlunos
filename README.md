Alunos: Bryan, Gustavo Lima, Kauan, Leciane e Thiago






PASSO 1 – Coloque a DLL na pasta correta

Abra a pasta do seu projeto no Windows Explorer, a mesma que contém as pastas src e out.

Crie a pasta:

libs

dentro dela crie outra pasta:

dll

A estrutura ficará assim:

Java Aluno EM
src
out
libs
dll
E1_Impressora01.dll

Copie o arquivo E1_Impressora01.dll e cole dentro da pasta dll.

PASSO 2 – Configurar o IntelliJ para reconhecer a DLL

No IntelliJ clique em Run e depois em Edit Configurations.

Na opção VM Options coloque:

-Djna.library.path=libs/dll
--enable-native-access=ALL-UNNAMED

Clique em Apply e depois OK.

Agora o IntelliJ passa a carregar a DLL corretamente.

PASSO 3 – Substituir o código antigo que carregava a DLL

Apague esta linha do seu código:

ImpressoraDLL INSTANCE = (ImpressoraDLL) Native.load("C:\Users\leciane_silva\Downloads\Java-Aluno EM\E1_Impressora01.dll", ImpressoraDLL.class);

E coloque isto no lugar:

static {
System.setProperty("jna.library.path", "libs/dll");
}

ImpressoraDLL INSTANCE = Native.load("E1_Impressora01", ImpressoraDLL.class);

Dessa forma a DLL é carregada automaticamente usando apenas o nome do arquivo, sem caminhos longos.

PASSO 4 – Corrigir o menu para evitar erro menos tres

Quando o menu pedir os dados, digite exatamente assim:

Tipo de conexao

Digite 1
O tipo 1 significa USB.

Modelo

Digite i9 ou o modelo correto da sua impressora.

Porta

Para impressora USB a porta correta é:

USB

Nao digite numeros como 3 ou 4 porque isso causa erro menos tres.

Parâmetro

Digite zero.

Exemplo completo

Digite o tipo de conexao: 1
Digite o modelo da impressora: i9
Digite a porta de conexao: USB
Digite o parametro adicional: 0

Depois escolha a opcao 2 para abrir conexao.
Se tudo estiver certo, aparecerá:

Conexao aberta com sucesso

Coisas que nao podem ser feitas

Nao digitar tipo igual a dois quando a impressora é USB
Nao digitar porta igual a quatro porque essa porta nao existe
Nao digitar modelo igual a tres
Nao tentar abrir conexao sem impressora conectada

PASSO 5 – Verificar se o Windows reconhece a impressora

Abra o Gerenciador de Dispositivos e procure por:

USB Printing Support
Elgin i9
Impressoras

Se nao aparecer nenhum desses itens, o erro menos tres vai continuar acontecendo.

PASSO 6 – Trecho completo e corrigido da interface ImpressoraDLL

Use exatamente este codigo:

public interface ImpressoraDLL extends Library {

static {  
    System.setProperty("jna.library.path", "libs/dll");  
}

ImpressoraDLL INSTANCE = Native.load("E1_Impressora01", ImpressoraDLL.class);

int AbreConexaoImpressora(int tipo, String modelo, String conexao, int param);
int FechaConexaoImpressora();
int ImpressaoTexto(String dados, int posicao, int estilo, int tamanho);
int Corte(int avanco);
int ImpressaoQRCode(String dados, int tamanho, int nivelCorrecao);
int ImpressaoCodigoBarras(int tipo, String dados, int altura, int largura, int HRI);
int AvancaPapel(int linhas);
int StatusImpressora(int param);
int AbreGavetaElgin(int i, int i1, int i2);
int AbreGaveta(int pino, int ti, int tf);
int SinalSonoro(int qtd, int tempoInicio, int tempoFim);
int ModoPagina();
int LimpaBufferModoPagina();
int ImprimeModoPagina();
int ModoPadrao();
int PosicaoImpressaoHorizontal(int posicao);
int PosicaoImpressaoVertical(int posicao);
int ImprimeXMLSAT(String dados, int param);
int ImprimeXMLCancelamentoSAT(String dados, String assQRCode, int param);


}

Pronto.
Com isso a DLL será carregada da pasta correta.
O aviso desaparece.
A conexao abrirá normalmente.
O erro menos tres só aparece se nao houver impressora conectada.
