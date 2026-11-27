### Preparando o Ambiente Virtual

crie um ambiente virtual py (.venv)

Com o ambiente ativado, instale as dependências necessárias e confirme se o sistema reconheceu o novo kernel:

    pip install jupyterlab kotlin-jupyter-kernel
    jupyter kernelspec list

### Atualizando o VS Code

para ativar o kernel recém instalado recarrege interface:

    Pressione as teclas Ctrl + Shift + P

    Digite e selecione a opção: Developer: Reload Window

### Selecionando o Kernel no Notebook

Com a janela recarregada e o ambiente .venv ainda ativo no terminal, configure o arquivo para rodar Kotlin:

Abra o seu arquivo notebook. Clique no botão de kernel localizado no canto superior direito da tela (geralmente exibindo .venv (Python)). No menu suspenso que aparecerá, clique em Select Another Kernel
Clique em Jupyter Kernel.Na lista final, selecione a opção Kotlin.

dentro de cada bloco de codigo as vezes vai ser preciso ir mudando para kotlin bloco de codigo por bloco para poder rodar.


