**Português (BR)** | [English (US)](CONTRIBUTING-en-US.md)


Guia de Contribuição
====


Este guia serve para orientar pessoas em sua jornada com o Querido Diário. Ele apresenta os passos iniciais que devem ser tomados ao interagir em todos os repositórios do projeto. 

Os repositórios também tem particularidades descritas em sua própria documentação. Ao finalizar a leitura destas orientações gerais, leia também o arquivo `CONTRIBUTING.md` do repositório de interesse antes de fazer sua contribuição. 

# Contribuindo

## Código de Conduta

Para garantir que a nossa comunidade seja acolhedora para todas as pessoas, leia e siga o nosso [Código de Conduta](CODE_OF_CONDUCT.md) em todos os espaços da comunidade.

Ao participar deste projeto, você concorda em aderir aos termos nele especificados.

## Ecossistema do Querido Diário

O Querido Diário possui vários repositórios, possibilitando contribuições onde houver mais afinidade:

- [Raspadores](https://github.com/okfn-brasil/querido-diario)
- [Frontend](https://github.com/okfn-brasil/querido-diario-frontend)
- [Documentação e Comunidade](https://github.com/okfn-brasil/querido-diario-comunidade)
- [Toolbox](https://github.com/okfn-brasil/querido-diario-toolbox)
- [Data Processing](https://github.com/okfn-brasil/querido-diario-data-processing)
- [API](https://github.com/okfn-brasil/querido-diario-api)
- [API Wrapper](https://github.com/okfn-brasil/querido-diario-api-wrapper)
- [Infraestrutura](https://github.com/okfn-brasil/querido-diario-infra)
- [Censo](https://github.com/okfn-brasil/censo-querido-diario)

## Como contribuir

Conhecer sobre `git` e como a plataforma *GitHub* funciona é um passo fundamental. Indicamos o [tutorial da Escola de Dados](https://escoladedados.org/tutoriais/introducao-ao-git-e-github-colaborando-com-projetos-de-codigo-aberto/) caso você ainda não conheça.

### Descobrindo com o que contribuir

#### Issues
Na aba **Issues** do repositório escolhido ficam as tarefas ou problemas específicos daquele repositório. Veja se uma te interessa.
- Quando a tarefa é indicada para iniciantes terá uma etiqueta com `good first issue` ("boa primeira tarefa").
- Ao encontrar uma que interessar, comente que fará a tarefa. Assim outras pessoas saberão que você já está trabalhando nela, podendo oferecer ajuda ou se dedicando a outras que ainda não tenham sido iniciadas.
- Já tendo, ou não, escolhido uma *issue* para trabalhar, você pode também comentar dúvidas relevantes relacionadas a tarefa.

#### Milestones
Se não houver *issues* abertas ou nenhuma te interessar, veja as **Milestones** do repositório de interesse. Elas indicam as metas a serem alcançadas. Explorar os arquivos buscando encontrar espaços de melhorias que ajudem o repositório a atingir uma *milestone* é uma excelente forma de contribuição.
- Ao encontrar, abra uma *issue* descrevendo a possibilidade de melhoria percebida. Se você já for realizar a tarefa, mencione na mensagem.

#### Discussions
Caso tenha uma ideia ou sugestão para os rumos do projeto, crie uma **Discussion** (discussão).
- Como o Querido Diário é composto por muitos repositórios e discussões tendem a ter caráter transversal, é **uma decisão de gestão manter todas elas centralizadas**. Assim, para iniciar ou acompanhar discussões, basta ver a aba [Discussion de `querido-diario-comunidade`](https://github.com/okfn-brasil/querido-diario-comunidade/discussions).

#### Chat para a comunidade
Ingresse na [comunidade](https://go.ok.org.br/discord). Lá você poderá tirar dúvidas mais rápidas, interagir com as pessoas mantenedoras, perguntar como poderia ajudar e ainda conversar com outras pessoas que também contribuem. 

### Interagindo com a pessoa revisora de código
- Tenha paciência e dê tempo para a revisora. Além de revisões de código serem complexas e demandarem, por si só, tempo e atenção, uma pessoa revisora pode ser, assim como você, outra contribuidora cedendo seu tempo pessoal quando possível ou ter outras prioridades antes de poder revisar seu código.
- Seja em forma de código ou com mensagens de dúvidas e sugestões, facilite a interação sendo objetivo(a), mas dando detalhes quando necessário.
- Durante a revisão, uma pessoa revisora pode abrir *threads* de solicitação de melhorias em seu código. Geralmente, é responsabilidade da pessoa revisora decidir quando a *thread* foi finalizada. 


# Mantendo

## Responsabilidades de uma pessoa mantenedora do Querido Diário

- Respeitar o [código de conduta](CODE_OF_CONDUCT.md) e garantir que as pessoas tenham um ambiente seguro e acolhedor e que qualquer vítima de infração desse termo tenha um canal de ajuda;
- Sempre justificar uma sugestão de acordo com as práticas já adotadas no projeto, legibilidade e simplicidade. É essencial que um projeto cívico tenha uma estrutura tão simples quanto possível para iniciantes;
- O projeto deve ser testado antes de um *Pull Request* ser mesclado;
- Manter o histórico de commits organizado, preferencialmente seguindo o formato a seguir onde toda alteração na base de código tem como base a main atualizada e é mesclada com um merge commit:

<p align="center">
  <a href="https://queridodiario.ok.org.br/sobre" target="_blank"> <img alt="História dos commits" src="./images/historia_commits.png">
  </a>
</p>

- Caso um Pull Request tenha muitos commits e as mensagens não forem claras, pode-se realizar um *squash* nos commits antes de mesclar o Pull Request



