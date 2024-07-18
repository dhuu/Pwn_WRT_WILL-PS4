# PPPwn no OpenWRT

Um método para executar o PPPwn em um roteador baseado em OpenWRT.

## Roteadores Suportado

Você pode verificar se o seu roteador é compatível [Aqui](https://openwrt.org/toh/start).

## Pré-requisitos

Depois que o OpenWRT estiver instalado, você precisará conectar temporariamente o roteador à Internet. Você pode fazer isso na interface web do LuCI seguindo estas etapas:

1. Acesse as configurações de **Sem fio** e selecione a opção **Scan**.
2. Selecione sua rede e junte-se a ela como cliente.

**AVISO:** Certifique-se de que sua interface `br-lan` (dhcp, rege de ips) não seja a mesma sub-rede que sua rede doméstica antes de ingressar como cliente. Caso contrário, eles entrarão em conflito.

## Configurar

Conecte-se remotamente ao seu roteador por meio de SSH

Baixe o projeto para o seu roteador:

```sh
opkg update
wget https://github.com/dhuu/Pwn_WRT_WILL-PS4/raw/main/install.sh
chmod +x install.sh && . ./install.sh
```

Selecione sua interface mais comum é `br-lan`.

Selecione seu firmware `11.00`, `10.00` or `9.00`.

Você será perguntado se deseja carregar o PPPwn da interface da Web. Você não deve usar esta opção se tiver espaço de armazenamento limitado e
estão planejando carregar o PPPwn na inicialização ou com um botão no roteador.

Você será perguntado se deseja carregar o script na inicialização. Se você selecionar `Y` para Sim, você poderá editar isso no futuro editando o arquivo: `/etc/rc.local`.

Você será perguntado se deseja desligar o roteador após carregar o exploit. Este recurso pode não funcionar em alguns roteadores e pode
fazer com que eles reiniciem. Se algum arquivo não for baixado corretamente durante a instalação, esta opção também poderá causar um loop de inicialização.

Você será perguntado se deseja instalar o nano. Se você tiver espaço de armazenamento limitado, é melhor recusar esta opção e usar o vi.

Agora você pode executar o script no terminal digitando `./run.sh` ou execute-o a partir da interface web do LuCI acessando **System > Custom Commands > PPPwn PS4 > Run**.

## Usando um botão para acionar a exploração 
(1-clique no botão WPS para executar "run.sh") ou Acione o processo killer (segure o botão WPS por 3 segundos para executar "kill.sh")

Este método é um pouco mais complicado que o anterior.

1. Entre no SSH do roteador e navegue até `cd /etc/rc.button`.

2. Digite `ls` para listar os botões disponíveis.
   ![Botões disponíveis](https://i.imgur.com/kb0hZrT.png)

3. Encontre um botão que deseja substituir, por exemplo, `wps`.

4. Edite o script do botão com:

    ```sh
    nano wps
    ```

5. Procure o código "released" do botão `wps`. Deve ser algo como:
   ![Código do botão WPS](https://i.imgur.com/ej8kr91.png)

6. Exclua tudo dentro da instrução `if` e substitua-o pelo seguinte comando:
   
    ```sh
    cd /root/PPPwn_WRT-main && ./run.sh
       if [ "$SEEN" -ge 3 ] ; then
          cd /root/PPPwn_WRT-main && ./kill.sh
       else
          cd /root/PPPwn_WRT-main && ./run.sh
       fi
    ```
    

  Exemplo:
   ![Script de botão](https://i.imgur.com/IMSN7Np.png)

Agora, ao pressionar o botão `wps`, o script será executado.

## Suporte LED

Se o seu roteador tiver indicadores LED para `wps`, `power`, `wlan`, etc., você pode usá-los para indicar quando o script está sendo executado.

1. Digite `ls /sys/class/leds/` para listar os LEDs disponíveis.
2. Escolha um LED, por exemplo, `red:info`.
3. Edite o script para alterar o comportamento do LED:

    ```sh
    nano /root/PPPwn_WRT-main/run.sh
    ```

4. Substitua `green:wps` por `red:info` em:

    ```sh
    echo "heartbeat" > /sys/class/leds/green:wps/trigger
    ```

Você também pode alterar o comportamento do LED de `heartbeat` para:
- `none` = desligado
- `default-on` = sempre ligado
- `heartbeat` = piscando
- `timer` = atraso de tempo
Baixar para linux/PI
https://github.com/dhuu/Pwn_WRT_WILL-PS4/raw/main/PI-Pwn-main.zip
---
