# BadUSB com Arduino Leonardo: Desativação da Proteção do SO

Prova de Conceito demonstrando um ataque de injeção de teclas (Keystroke Injection) para desabilitar o Microsoft Defender em segundos, explorando o acesso físico a uma máquina.

### 🎥 [Link para o Vídeo de Demonstração](https://youtu.be/vkh13efq-kM?si=Tz2PhtAMAF8ejYz5)

---

> ### ⚠️ AVISO ÉTICO
> Este projeto foi desenvolvido para fins estritamente educacionais e de demonstração de vulnerabilidades. O objetivo é conscientizar sobre os perigos associados ao acesso físico não autorizado a dispositivos. Todas as ações foram realizadas em um ambiente controlado e de minha propriedade. A replicação deste experimento em sistemas alheios sem consentimento explícito é ilegal e antiética. Utilize este conhecimento para construir defesas, não para atacar.

---

### Resumo do Projeto (Relatório de Ameaça)
Este projeto é uma prova de conceito (PoC) de um ataque **BadUSB**, que explora a confiança inerente que os sistemas operacionais depositam em Dispositivos de Interface Humana (HID). O Arduino Leonardo, programado para emular um teclado e mouse, executa um ataque de **injeção de teclas** para navegar pela interface do Windows e desativar a proteção em tempo real do Microsoft Defender. A demonstração evidencia como um acesso físico de poucos segundos a uma máquina desbloqueada pode ser suficiente para comprometer suas defesas primárias, deixando-a vulnerável a malwares e outras ameaças.

---

### Documentação Técnica (Análise do Ataque)

#### **Vetor de Ataque**
* **Injeção de Teclas (Keystroke Injection)** via dispositivo HID malicioso. O sistema operacional não consegue distinguir os comandos enviados pelo Arduino dos comandos de um teclado humano.

#### **Objetivo (Payload)**
* Desabilitar a "Proteção em Tempo Real" do Microsoft Defender no Windows 11.

#### **Hardware Necessário**
* Arduino Leonardo (ou similar com ATmega32U4, como o Pro Micro)
* Cabo USB

#### **Software e Tecnologias**
* Linguagem: C/C++ (Arduino Framework)
* IDE: Arduino IDE
* Bibliotecas: `Keyboard.h`, `Mouse.h`

#### **Execução do Ataque (Análise do Payload)**
O *payload* é executado na função `setup()`, garantindo que a ação ocorra imediatamente após o dispositivo ser conectado e reconhecido.

1.  **Acesso Inicial:** O ataque começa com o atalho `Win + I` para abrir o painel de "Configurações".
2.  **Navegação Cega:** A navegação é feita primariamente com comandos de teclado (`TAB`, `SETA PARA BAIXO`, `ENTER`).
3.  **Localização do Alvo:** A sequência de comandos foi calculada para navegar até "Privacidade e segurança" > "Segurança do Windows".
4.  **Desativação da Proteção:** O script utiliza uma combinação final de teclado e mouse para alternar o *switch* que desativa a proteção em tempo real.

#### **Desafios e Contramedidas**

##### **Desafios (Perspectiva do Atacante):**
* **Timing:** Calibrar os `delays` foi desafiador. Um `delay` muito curto faz com que o comando seja enviado antes que a UI responda, quebrando o script.
* **Variações de UI:** O script é sensível a atualizações do sistema operacional que alterem a ordem ou a estrutura dos menus de configuração.

##### **Contramedidas (Perspectiva do Defensor):**
* **Segurança Física:** Bloquear a tela (`Win + L`) sempre que se afastar do computador. Esta é a defesa mais eficaz.
* **Controle de Portas USB:** Em ambientes corporativos, softwares de *whitelisting* podem impedir que dispositivos USB não autorizados sejam executados.
* **Conscientização do Usuário:** Treinar usuários para não conectarem dispositivos USB de origem desconhecida.
