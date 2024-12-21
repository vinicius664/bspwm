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

<h1></h1>


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
<h3></h3>


