# bspwm
Config file

<h1>Silenciar o grub:</h1>

<h3>Abra o arquivo:</h3>
<pre># sudo nano /etc/default/grub</pre>

<h3>GRUB_DEFAULT e GRUB_TIMEOUT deixe o valor como 0:</h3>
<pre>GRUB_DEFAULT=“0”<br>
GRUB_TIMEOUT=“0”</pre>

<h3>Embaixo delas adicione a seguinte linha:</h3>
<pre>GRUB_RECORDFAIL_TIMEOUT=$GRUB_HIDDEN_TIMEOUT</pre>

<h3>Na linha GRUB_CMDLINE_LINUX_DEFAULT adicione:</h3>
<pre>quiet loglevel=3 rd.systemd.show_status=auto rd.udev.log_priority=3<br>vt.global_cursor_default=0 vga=current</pre>

<h3>Resultado:</h3>
<pre>GRUB_CMDLINE_LINUX_DEFAULT=(quiet loglevel=3 rd.systemd.show_status=auto rd.udev.log_priority=3 vt.global_cursor_default=0 vga=current)</pre>

<h3>Na linha GRUB_TIMEOUT_STYLE, mude o valor para hidden:</h3>
<pre>GRUB_TIMEOUT_STYLE=“hidden”</pre>

<h3>Embaixo dela adicione a seguinte linha:</h3>
<pre>GRUB_HIDDEN_TIMEOUT=3</pre>

<h3>Salve o arquivo e depois rode o seguinte comando:</h3>
<pre># sudo grub-mkconfig -o /boot/grub/grub.cfg</pre>

<h1>Remover as mensagens do sysctl:</h1>
<pre># sudo nano /etc/sysctl.d/20-quiet-printk.conf</pre>

<h3>Adicione a seguinte linha:</h3>
<pre>kernel.printk = 3 3 3 3</pre>

<h3>Deixar o SystemD verificar o sistema de arquivos:</h3>
<pre># sudo nano /etc/mkinitcpio.conf</pre>

<h3>Na linha HOOKS, apague udev e coloque no lugar systemd:</h3>
<pre>HOOKS=( base systemd fsck …)</pre>

<h3>Salve o arquivo e rode o seguinte comando:</h3>
<pre># sudo mkinitcpio -p linux</pre>

<h1>Remover mensagens desnecessárias do systemd:</h1>
<pre># systemctl edit --full systemd-fsck-root.service<br>
# systemctl edit --full systemd-fsck@.service</pre>

<h3>Adicione as seguintes linhas abaixo de ExecStart configurando StandardOutput e StandardError assim:</h3>
<pre>StandardOutput=null<br>StandardError=journal+console</pre>

<h3>Salve o arquivo e depois rode esse comando:</h3>
<pre># sudo systemctl edit --full systemd-fsck@.service</pre>

<h3>Remover mensagem de inicialização:</h3>
<pre># sudo pacman -S grub-customizer</pre>

<h3>Abra o programa, clique na entrada Arch Linux e clique em editar e apague todas as linhas com echo:</h3>
<pre>echo ‘Loading Linux linux …’<br>
echo ‘Loading initial ramdisk …’</pre>

<h3>Resolver erro de tradução grub:</h3>
<pre># sudo cp /usr/share/locale/en@quot/LC_MESSAGES/grub.mo /boot/grub/locale/en.mo</pre>

<h1>Instalando e configurando o bspwm</h1>

<h3>/Instale o xorg:</h3>
<pre># sudo pacman -S xorg xorg-server xorg-xinit</pre>

<h3>Instale os drivers:</h3>
<pre># pacman -Ss xf86-video</pre>

<h3>Instalando:</h3>
<pre># sudo pacman -S bspwm sxhkd xfce4-terminal</pre>

<h3>Copiando os arquivos de configuração e criando os diretórios:</h3>
<pre>
# mkdir -p ~/.config/{bspwm,sxhkd}<br>
# cp /usr/share/doc/bspwm/examples/bspwmrc ~/.config/bspwm<br>
# cp /usr/share/doc/bspwm/examples/sxhkdrc ~/.config/sxhkd
</pre>

<h3>Inicializando o bspwm, pode editar o arquivo direto:</h3>
<pre>
# sudo nano /etc/X11/xinit/xinitrc
</pre>

<h3>Ou pode fazer uma cópia no seu diretório:</h3>
<pre>
# sudo cp /etc/X11/xinit/xinitrc ~/.xinitrc
</pre>

<h3>Adicione as seguintes linhas no final do xinitrc: </h3>
<pre>
# sudo nano ~/.xinitrc OU sudo nano /etc/X11/xinit/xinitrc</pre>
<pre>
...
setxkbmap br &
exec bspwm</pre>

<h3>Abra o arquivo de configuração do bspwm:</h3>
<pre># sudo nano ~/.config/bspwm/bspwmrc</pre>

<h3>No inicio do arquivo de configuração adicione a seguinte linha:</h3>
<pre>
sxhkd &
wmname LG3D &
</pre>

<h3>Para o mouse aparecer, use o seguinte comando no inicio do arquivo de configuração bspwmrc:</h3>
<pre>xsetroot -cursor_name left_ptr &</pre>

<h3>No arquivo sxhkdrc, faça a segunte edição:</h3>
<pre>
Onde tem:<br>
super + Return
urxvt<br>
Modifique para:<br>
super + Return
xfce4-terminal
</pre>
<p>A tecla super é correspondente a tecla "Windows", e a tecla return corresponde a tecla "Enter".</p>
<p>Então Windows + Enter: abre o xfce4-terminal.</p>

<h3>Para iniciar o bspwm:</h3>
<pre>
# startx
</pre>

<h3>Configurações:</h3>
<pre>
Entendendo:
#!/bin/sh

sxhkd &
(Os serviços que quiser que inicie junto com o WM, podem ser incluídos aqui.)

bspc monitor -d I II III IV V VI VII VIII IX X
(O nome das workspaces, pode renomear como quiser cada um deles)

bspc config border_width 2
bspc config window_gap 12
bspc config split_ratio 0.52
bspc config borderless_monocle true
bspc config gapless_monocle true
(Aqui vem as configurações das bordas e gaps.)

bspc rule -a Gimp desktop=’^8’ state=floating follow=on
bspc rule -a Chromium desktop=’^2’
bspc rule -a mplayer2 state=floating bspc rule -a
Kupfer.py focus=on bspc rule -a Screenkey manage=off
(Regras para as janelas. É aqui que se define em qual workspace a janela será aberta, e o comportamento da mesma.)
Podemos adicionar algumas configurações extras, como:

bspc config focus_follows_pointer true
(O foco da janela segue o cursor.)

bspc config normal_border_color "#528588"
bspc config focused_border_color "#dee3e0"
bspc config presel_feedback_color “#2c3939”
(Cores.)

Regras.
As regras podem ser confusas para alguns, então irei explicar seu funcionamento.
Para definir uma regra para uma janela, você precisa saber o nome da sua classe, para descobrir isso, podemos usar o pacote xprop:

$ xprop | awk '/WM_CLASS/{print $4}'
Após rodar o comando, clique na janela que deseja saber a classe.

Tendo o nome da sua classe, podemos começar a criar nossa regra. Vamos pegar o terminal st de exemplo:

bspc rule -a st-256color desktop=‘^1’
No exemplo acima, o terminal st irá abrir no desktop 1. Vamos continuar…

bspc rule -a st-256color desktop=‘^1’ follow=on focus=on
Uma coisa interessante do bspwm, é este “follow=on”, que se você estiver em um desktop diferente do 1,
ela irá te levar até o 1. Já o “focus=on”, irá focar na janela, caso tenha várias janelas abertas no mesmo desktop.

Também podemos adicionar “center=true”, para um programa com a regra floating abrir no centro da tela.
</pre>

<h1>Instalado os arquivos:</h1>
<pre># git clone https://github.com/vinicius664/bspwm.git</pre>

<h3>Coloque os arquivos nos diretórios corretos:</h3>
<pre>
cd bspwm
cp bspwm/backgrounds/*.jpg,.png usr/share/backgrounds/
sudo mkdir /usr/share/fonts/{TTF,OTF}
cp bspwm/fonts/*.ttf /usr/share/fonts/TTF/
cp bspwm/fonts/*.otf /usr/share/fonts/OTF/
</pre>

<h1>Instalando o polybar</h1>
<pre># sudo pacman -S polybar</pre>

<h3>Copie o exemplo de configuração:</h3>
<pre>
# mkdir ~/.config/polybar
# cp /usr/share/doc/polybar/examples/config.ini ~/.config/polybar/config
</pre>

<h3>Inicializando o polybar:</h3>
<p>Crie um arquivo script executável contendo a lógica de inicialização em ~/.config/polybar/launch.sh:</p>
<pre>
# sudo nano ~/.config/polybar/launch.sh
</pre>
<p>No arquivo insira:</p>
<pre>
#!/bin/bash
  
#Termine instâncias de barras em execução
killall -q polybar

#Espere até que os processos em execução sejam terminados
while pgrep -u $UID -x polybar >/dev/null; do sleep 1; done

#execute a Polybar, usando a configuração padrão ~/.config/polybar/config
polybar NOMEDABARRADOPOLYBAR &

echo "Polybar lançada..."
</pre>

<p>Ative o script como inicializável:</p>
<pre>sudo chmod +x ~/.config/polybar/launch.sh</pre>

<p>No bspwmrc insira o seguinte comando: </p>
<pre>
${HOME}/.config/polybar/launch.sh &
</pre>

<h3></h3>
<h3></h3>
<h3></h3>
<h3></h3>
<h3></h3>
<h3></h3>
<h3></h3>
<h3></h3>
<h3></h3>
<h3></h3>
<h3></h3>
<h3></h3>
<h3></h3>
<h3></h3>
<h3></h3>
<h3></h3>


