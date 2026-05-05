# HA de SSH

L'alta disponibilitat d'SSH no és de dades, sinó **d'accés**.

!!! info "Idea"
    - `sshd` actiu als dos nodes
    - Configuració semblant
    - Connexió sempre a la `VIP`

## 1. Revisar que SSH estigui actiu

Als dos servidors:

```bash
sudo systemctl status ssh
```

## 2. Sincronitzar configuració

Al `srv-primari`, sincronitzar si cal:

```bash
sudo rsync -av /etc/ssh/ root@192.168.0.101:/etc/ssh/
```

![Sincronització SSH](../assets/img/lab/image-15.png)

## 3. Reiniciar als dos servidors

```bash
sudo systemctl restart ssh
```

![Reinici SSH](../assets/img/lab/image-16.png)

## 4. Provar per la VIP

Des del PC:

```bash
ssh alumne@192.168.0.110
```

![Connexió SSH via VIP](../assets/img/lab/image-17.png)

## 5. Comprovar failover

Apaguem el primari:

![Failover SSH](../assets/img/lab/image-18.png)

Com podem observar, el secundari agafa la IP del primari i ens podem connectar.

!!! warning "Nota important"
    Si els dos servidors tenen claus host diferents, quan hi hagi failover pot sortir l'avís de canvi d'identitat SSH. Això no vol dir que estigui malament, però cal saber-ho explicar.
