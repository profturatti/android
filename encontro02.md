# Preparando o ambiente de desenvolvimento em Windows 11

:warning: Para realizar os passos a seguir é necessário ter acesso ao sistema como administrador

## 1. Abra um terminal PowerShell com privilégio administrativo

Clique na barra de pesquisa (Pesquisar) ao lado do botão iniciar e digite: `powershell`

Dentre as opções disponíveis na lista

Clique na opção "Executar como administrador"

Se o usuário que estiver usando não for do grupo adminsitrador, informe o usuário com privilégio administrativo e respectiva senha

## 2. Ajustando as permissões necessárias

Agora temos uma janela com título: "Administrador: Windows PowerShell" disponível.

É nessa janela que executaremos os demais comandos

1. Verifique a configuração das permissões
```
> Get-ExecutionPolicy
```

Se aparecer "Restricted" então digite:
```
> Set-ExecutionPolicy AllSigned

> Get-ExecutionPolicy
AllSigned
```
Isto confirma que podemos prosseguir

## 3. Instalando o chocolatey

Copie e cole o conteúdo a seguir no terminal powershell e pressione ENTER
```
Set-ExecutionPolicy Bypass -Scope Process -Force; [System.Net.ServicePointManager]::SecurityProtocol = [System.Net.ServicePointManager]::SecurityProtocol -bor 3072; iex ((New-Object System.Net.WebClient).DownloadString('https://chocolatey.org/install.ps1'))
```

Após alguns instantes a instalação é concluída.

Verifique o sucesso desta etapa com:
```
> choco --version
2.3.0
```

## 4. Instalando o Node.JS

```
> choco install nodejs-lts -y
Installing ...
...
Chocolatey installed 1/1 packages.
 See the log for details (C:\ProgramData\chocolatey\logs\chocolatey.log).
```
Verifique a versão instalada. 
No momento da elaboração deste tutorial, a versão LTS mais recente é mostrada abaixo:
``` 
> node --version
v20.17.0
```

## 5. Instalando o Java 11
```
> choco install -y openjdk11
```
Verifique a versão instalada.
```
> java --version
openjdk 11.0.2 2019-01-15
OpenJDK Runtime Environment 18.9 (build 11.0.2+9)
OpenJDK 64-Bit Server VM 18.9 (build 11.0.2+9, mixed mode)
```

## 6. Instalando o Git
```
> choco install git.install
Installing...
...
choco feature enable -n allowGlobalConfirmation
Do you want to run the script?([Y]es/[A]ll - yes to all/[N]o/[P]rint): Y
...
Chocolatey installed 3/3 packages.
 See the log for details (C:\ProgramData\chocolatey\logs\chocolatey.log).
```
Verifique a versão instalada.
```
> git --version
git version 2.46.0.windows.1
```

## 7. Instalando o Python 3.12
```
> choco install python312 -y
Installing...
...
Chocolatey installed 9/9 packages.
 See the log for details (C:\ProgramData\chocolatey\logs\chocolatey.log).

Installed:
 - chocolatey-windowsupdate.extension v1.0.5
 - KB2919355 v1.0.20160915
 - KB2919442 v1.0.20160915
 - KB2999226 v1.0.20181019
 - KB3033929 v1.0.5
 - KB3035131 v1.0.3
 - python312 v3.12.5
 - vcredist140 v14.40.33810
 - vcredist2015 v14.0.24215.20170201

Packages requiring reboot:
 - vcredist140 (exit code 3010)

The recent package changes indicate a reboot is necessary.
 Please reboot at your earliest convenience.
```
Podemos listar os pacotes instalados com o seguinte comando:
```
> choco list python
Chocolatey v2.3.0
 chocolatey 2.3.0
 python312 3.12.5
2 packages installed.
```
Reinicie o computador. A melhor forma de fazer isto é desligando a máquina, aguardar uns instantes e ligar novamente.

Após a conclusão da inicialização, repita o passo 1 para ter um terminal PowerShell com acesso administrativo.

Verifique a versão instalada.
```
> python --version
Python 3.12.5
```

## 8. Intalando o VSCode 1.92.2
```
> choco install vscode -y
> code --version
```

## 9. Desenvolvendo seu projeto

Para a disciplina de desenvolvimento móvel com Android temos dois caminhos possíveis para fazer isso no computador pessoal.

1. Utilizar o Android Studio (emulador requer ao menos 16GB RAM). No chocolatey o pacote é `Android Studio Koala | 2024.1.1 (Requer 1.12GB v2024.1.1.13)`
```
> choco install AndroidStudio -y       
> studio64

Aceitar as condições e concluir a instalação
```

-ou-

2. Utilizar o EXPO (requer 4GB RAM para utilizar o expo-cli) e em seu dispositivo móvel é necessário instalar o Expo-GO.

## 10. Comandos úteis do chocolatey

Exemplo para pesquisar programas para instalar com chocolatey
```
> choco search google chrome
```
Exemplo para listar programas já instalados com chocolatey
```
> choco list
```
