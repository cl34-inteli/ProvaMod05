# ProvaMod05
## Regras de negócio

### Requisitos
- O contrato deve armazenar as jogadas dos jogadores.
- As jogadas devem ser registradas e garantir integridade dos dados.
- Deve existir um mecanismo para comparar as jogadas e determinar o vencedor (ou empate).
- Apenas participantes legítimos podem jogar.
- O dono do contrato deve ter controle de acesso para modificar dados críticos.
  
### Regras
- Pedra (1) vence Tesoura (3)
- Tesoura (3) vence Papel (2)
- Papel (2) vence Pedra (1)
- Se empate, você deve jogar de novo.

Versão do compilador descrita em "compilation details" em "Solidity compiler" na sidebar posicionada ao lado esquerdo da tela: version:0.8.26+commit.8a97fa7a
```
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.22;

contract Jokenpo {
    address public jogador1;
    address public jogador2;
    uint public jogada1; // 1 = Pedra, 2 = Papel, 3 = Tesoura
    uint public jogada2; // 1 = Pedra, 2 = Papel, 3 = Tesoura
    string public resultado;

    constructor(address _jogador1, address _jogador2) { //constructor para armazenar os jogadores
        jogador1 = _jogador1;
        jogador2 = _jogador2;
    }

    function registrarJogada(uint jogada) public { //função para registrar as jogadas
        require(jogada >= 1 && jogada <= 3, "Jogada invalida. Use 1, 2 ou 3."); //aqui define o range de 1 a 3 e aviso de número errado
        if (msg.sender == jogador1) { //define que a jogada 1 pertence ao jgador 1 e a 2 ao jogador 2 e só permite com adress definido
            jogada1 = jogada;
        } else if (msg.sender == jogador2) {
            jogada2 = jogada;
        } else {
            revert("Apenas jogadores registrados sao permitidos!"); //aviso de endereço não cadastrado
        }

        if (jogada1 > 0 && jogada2 > 0) { 
            determinarResultado();
        }
    }

    function determinarResultado() private { //determina os possíveis finais do jogo contemplando as regras
        if (jogada1 == jogada2) {
            resultado = "Empate! Tente outra vez. XD"; // aviso de empate
        } else if (
            (jogada1 == 1 && jogada2 == 3) || 
            (jogada1 == 2 && jogada2 == 1) || 
            (jogada1 == 3 && jogada2 == 2)
        ) {
            resultado = "Jogador 1 venceu!";
        } else {
            resultado = "Jogador 2 venceu!";
        }
    }
}
```

### Processo de compilação, deploy e uso:
1. Abra o site Remix IDE (https://remix.ethereum.org/#lang=en&optimize=false&runs=200&evmVersion=null&version=soljson-v0.8.26+commit.8a97fa7a.js);
2. Em file explorer clique com o botão direito sobre a pasta "contracts" e crie um novo arquivo;
3. Nomeie esse aquivo de "jokenpo.sol";
4. Copie o código acima;
5. Cole ele dentro do arquivo;
6. Na sidebar no lado esquerdo da tela você vai encontrar uma seção chamada "Solidity compiler", você deverá acessá-la (é possível ler o ícone passando o mouse por cima);
7. Se a versão do compilador for diferente da citada acima nessa sessão, você deverá substituí-la pela mencionada nesse arquivo;
8. Clique em compilar;
9. Para jogar jokenpo com o smart contract você deverá fazer um deploy;
10. Para isso, conecte-se a sua metamask;
11. Certifique-se que está conectado à uma testnet como a Sepolia;
12. Verifique se possui faucets o suficiente para as transações;
13. Acesse a seção chamada "Deploy & run transactions";
14. Coloque o endereço da sua carteira e da carteira de quem irá jogar com você;
15. Selecione quem irá fazer a jogada (jogador 1 ou 2);
16. Coloque o número equivalente a sua jogada (1 para pedra, 2 para papel e três apra tesoura);
17. Clique em resultaod e descubra quem venceu!
