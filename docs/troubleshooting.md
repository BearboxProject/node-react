## Ambiente de desenvolvimento

### Lint-staged

Ao tentar realizar um commit no Git, você pode encontrar erros relacionados ao lint-staged, impedindo a conclusão do processo de commit. Esse problema geralmente está associado à execução de testes e verificações de formatação antes de cada commit. Ferramentas como `Jest` (para testes), `ESLint` (para padrões de código) e `Prettier` (para formatação) são automaticamente acionadas. Se qualquer uma dessas ferramentas detectar um problema, o lint-staged, responsável por gerenciar essas verificações, falhará, resultando no erro mencionado.

Solução:

- Execute os testes manualmente para garantir que todos estejam passando. **Qualquer** checagem falhando pode causar o erro.
- Garanta que o seu código esteja em conformidade com as regras do ESLint. Variáveis e imports não utilizados são as principais causas de erros.
- Assegure-se de que o seu código esteja formatado corretamente de acordo com as configurações do Prettier.

Para prevenir esses problemas de forma proativa, é altamente recomendável utilizar as **extensões** ESLint e Prettier em seu ambiente de desenvolvimento. Estas extensões ajudam a identificar e corrigir problemas de formatação e padrões de código em tempo real, reduzindo significativamente as chances de erros durante o processo de commit, além de aplicar a formatação automaticamente ao salvar os arquivos.

## Componentes

### Margin e padding

Ao encadear estilos, o esperado é que o estilo mais recente substitua o anterior. No entanto, isso não parece acontecer com propriedades específicas de margem. Quando se define marginBottom em um estilo e, posteriormente, se aplica um estilo com margin, a marginBottom original não é substituída. Isso leva a um comportamento inesperado na interface do usuário.

Este comportamento ocorre devido à maneira como o React Native lida com estilos específicos de margem em relação aos estilos de margem genéricos. Quando você aplica marginBottom, o React Native o trata como uma propriedade mais específica em comparação com a propriedade margin genérica. Isso significa que, mesmo que margin seja definida posteriormente, ela não substituirá marginBottom, pois o React Native dá prioridade a estilos mais específicos.

Solução:

- Garanta que todas as margens específicas sejam explicitamente definidas no último estilo aplicado. Por exemplo, se você estiver aplicando marginBottom e, em seguida, quiser aplicar margin, certifique-se de que o último estilo também especifique marginBottom.

```jsx
const style1 = { marginBottom: 10 };
const style2 = { margin: 5, marginBottom: 5 }; // marginBottom tem prioridade, independentemente da ordem
```