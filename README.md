# Clustered Hadoop

Este projeto tem como objetivo exercitar o setup do [Hadoop](https://hadoop.apache.org/) em cluster .<br/>
Não é propósito deste laboratório substituir a documentação disponível sobre o tema. A mesma é clara e objetiva e pode ser encontrada [aqui](https://hadoop.apache.org/docs/r3.1.2/).

O conteúdo do laboratório possui algumas imagens Linux que sobem via [Vagrant](https://github.com/infobarbosa/clustered-hadoop/blob/master/Vagrantfile):
- 1 master;
- 3 datanodes.

> _Antes de iniciar_
> É necessário que os hosts tenham uma relação de confiança estabelecida. 
> Já deixei esse setup pronto via automação Vagrant, sua parte é disponibilizar um par de chaves (pública e privada).<br>
```
ssh-keygen -q -t rsa -b 4096 -C "hadoop@infobarbosa.github.com" -P "" -f "./config-files/master_key"
```

By the way, se não estiver familiarizado com o Vagrant, sugiro começar por [aqui](https://www.vagrantup.com/intro/index.html).<br/>

Para iniciar os boxes basta digitar:
```
vagrant up
```

A montagem dos boxes leva entre 15 e 30 minutos, a depender da máquina que você tem. Vá tomar um café!


Para destruir o ambiente no Vagrant é muito simples:
```
vagrant destroy -f
```

> Por favor, fique à vontade pra me mandar suas dúvidas, críticas e sugestões. Ajudarei no que estiver ao meu alcance. ;)

Abraço,

Barbosa @infobarbosa
