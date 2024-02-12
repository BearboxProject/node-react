## Definição

Em React, os componentes são essenciais para a construção de interfaces de usuário complexas, pois dividem a aplicação em partes menores e mais gerenciáveis. Embora seja fácil entender essa ideia, é importante ressaltar que a definição de componentes abrange uma ampla gama de aplicações. Neste contexto, podemos defini-los de maneira mais precisa: componentes são todas as funções que retornam JSX.

Isso significa que qualquer bloco de código semelhante ao exemplo abaixo é considerado um componente:

```jsx
export function App() {
  return <div>Hello from App</div>;
}
```

Assim, torna-se possível categorizar diversos elementos, tais como telas, templates e formulários, como componentes. Contudo, embora compartilhem semelhanças, é crucial destacar que não são idênticos. Por esse motivo, ao organizar o código, a abordagem vai além da simples classificação por telas. Torna-se valioso categorizar os componentes com base em seus nomes e funções lógicas, reconhecendo a singularidade e a contribuição específica de cada um para a arquitetura geral da aplicação.

```tsx
screen / nome_da_tela.tsx;
components / botão.tsx;
```

No exemplo acima, embora tenhamos algumas divisões de componentes simples, como telas, ainda não alcançamos um nível de atomicidade que nos permita agrupar códigos de maneira completamente independente.

Idealmente, todos os componentes do mesmo tipo deveriam ser tratados de maneira uniforme, mas mesmo com essa organização, alguns algoritmos podem se destacar em relação a outros. Por exemplo, um formulário, devido a sua importância lógica e complexidade, merece uma abordagem distinta comparada a um botao. Sendo assim, nós utilizamos outras categôrias para fazer divisões ainda menóres.

## Tipos de componentes

### Common

Os Componentes Comuns, como o próprio nome sugere, são elementos fundamentais em React, projetados para tarefas simples e não requerem um nome ou atribuição especial. Sua simplicidade é o que os torna a base essencial para a construção de interfaces interativas e agradáveis.

Características Distintivas:

- Foco em elementos visuais e interatividade.
- Podem ser estado-dependentes (stateful) ou independentes (stateless).
- Geralmente servem como wrappers para elementos nativos de HTML e outros componentes de apresentação.

```jsx
import { useState } from "react";

const Botao = ({ texto, onClick }) => {
  const [contagemCliques, setContagemCliques] = useState(0);

  return (
    <button
      onClick={() => {
        onClick();
        setContagemCliques(contagemCliques + 1);
      }}
    >
      {texto} - Cliques: {contagemCliques}
    </button>
  );
};

export default Botao;
```

### Layout

Estes componentes concentram-se em estruturar e organizar o espaço da interface de usuário. Sua função é puramente estrutural, não contendo lógica de negócios, mas ajudando a manter um design responsivo e consistente.

Características Distintivas:

- Fornecem uma estrutura para outros componentes sem gerenciar estado ou lógica.
- Exemplos incluem wrappers para grids, espaçamento e alinhamento.

Os exemplos acima são, em essência, tipos de containers. Esses geralmente possuem alguma margin e padding e servem principalmente para alinhamento de objetos filhos.

```jsx
const Container = ({ children, margin, padding }) => (
  <div
    style={{
      margin: margin || "10px",
      padding: padding || "15px",
    }}
  >
    {children}
  </div>
);
```

No sistema atual, além do Container temos um layout extremamente importate: a Surface. Sua responsabilidade é criar uma elevação e efeitos de sombras e bordas. Esse layout da aos botões, inputs, cards e listas o aspecto de superficie elevada enquanto mantém a consistência ao longo de toda a aplicação.

### Composite

Composite não é apenas um tipo especial de componente, mas sim vários. Trata-se de uma convenção básica que lida com componentes de maior complexidade, os quais, em outras situações, poderiam ter suas próprias pastas, mas acabariam apenas aumentando a complexidade dos arquivos.

Podemos definir um composite como um componente complexo que depende de vários componentes simples. Um bom exemplo seria um formulário.

```jsx
function LoginScreen() {
  return (
    <div>
      <form>
        <UsernameInput />
        <PasswordInput />
        <SubmitButton />
      </form>
    </div>
  );
}
```

Ao invés de criarmos um formulário diretamente na tela de login, poderíamos criar um LoginForm, cujo qual estaria responsável apenas pela renderização e organização dos componentes. Ele seria um container enquanto o elemento pai, a tela de login, lida com tarefas lógicas mais complexas como validação e envio.

```jsx
function LoginForm() {
  return (
    <form>
      <UsernameInput />
      <PasswordInput />
      <SubmitButton />
    </form>
  );
}
```

```jsx
function LoginScreen() {
  return (
    <div>
      <LoginForm />
    </div>
  );
}
```

A principio a diferença não é grande e parece ser mais um incomodo do que um ajuda, porém a coisa muda de figura quando começar a tratar da implementação da lógica do formulário. Aqui vemos claramente onde uma divisão de conceitos pode ajudar a organizar o código de forma clara e concisa.

```jsx
function LoginForm({ control, errors, onSubmit }) {
  return (
    <form>
      <UsernameInput control={control} error={errors.username?.messsage} />
      <PasswordInput control={control} error={errors.password?.message} />
      <SubmitButton onSubmit={onSubmit} />
    </form>
  );
}
```

```jsx
function LoginScreen() {
  // um hook personalizado criado em cima do useForm
  const { control, errors, handleSubmit } = useFormControl({
    resolver: yupResolve(loginSchema),
  })

  const handleFormSubmit = (data) => { ... }

  return (
    <div>
      <LoginForm
        control={control}
        errors={errors}
        onSubmit={handleSubmit(handleFormSubmit)}
      />
    </div>
  );
}
```

Outro exemplo seria uma função FormGroup. Ela é composta por um input, um label e um local para mostrar erro, entre outros elementos. Embora seja simples o suficiente para ser classificada como uma função comum, pode ter dependências o suficiente para se tornar uma função composite. Tudo depende da implementação e da decisão do programador.

```jsx
// A função FormGroup pode ser considerada uma função composite, pois encapsula a lógica e a estrutura de vários elementos relacionados.

function FormGroup({ label }) {
  return (
    <div>
      <label>{label}</label>
      <CustomInput type="text" />
      <ErrorDisplay />
    </div>
  );
}
```

Por fim, é importante deixar claro que composite não é um padrão por definição. Seu uso é interpretativo e pode causar algumas confusões a primeira vista, mas seu objetivo principal permanece como o de separar componentes complexos de componentes mais simples.

## Dependências

### Context

A pasta "context", como o próprio nome sugere, armazena os contextos do React. Isso será especialmente útil na seção de padrões, mas existem algumas aplicações evidentes, como no fluxo de autenticação. Ela deve incluir os seguintes itens:

```sh
context/
|-- customContext/
|   |-- index.tsx
|   |-- customContext.context.tsx
|   |-- customContext.hook.tsx
|   |-- customContext.provider.tsx
```

A decisão de colocar o hook que utiliza o contexto dentro de sua pasta é um pouco arbitrária, mas consideramos que agrupar arquivos semelhantes facilita a manutenção e o desenvolvimento.

### Hooks

Os hooks devem ser compostos por apenas um arquivo, conforme seu próprio nome. Além disso, o arquivo deve exportar apenas sua função correspondente.

```js
export useCustomHook01() {
  ...
}
```

```sh
hooks/
|-- useCustomHook01.tsx/
|-- useCustomHook02.tsx/
|-- useCustomHook03.tsx/
```

Não existe uma necessidade real de usar a extensão `.tsx`. Apenas seja coerente.

## Padrões

### Import/Export

O método padrão para a exportação de elementos como hooks, contexts, serviços ou dependências é o uso de exportação **nomeada**. Isso implica que a maioria das funções e classes deve evitar o uso do export default.

> Na seção seguinte trataremos de um caso especial, o de componentes.

Considere o exemplo abaixo para um hook personalizado, useCustomHook:

```js
// useCustomHook.js
const useCustomHook = () => {
  // lógica do hook
};

const outraFuncao = () => {
  // outra funcionalidade relacionada ao hook
};

export { useCustomHook, outraFuncao };
```

> O export deve ser preferencialmente feito no fim da página ou após alguma seção importante de código.

A mesma prática se estende para enums, interfaces, objetos e outras estruturas de dados. Ao adotar a exportação nomeada para esses elementos, proporcionamos uma clareza adicional sobre a estrutura do código e facilitamos a compreensão de suas propriedades.

```ts
interface Usuario {
  nome: string;
  idade: number;
}

const configuracoes = {
  idioma: "portugues",
  temaEscuro: true,
};

export { configuracoes };
export type { Usuario };
```

> No caso de interfaces e types, o export deve ser feito utilizando a keyword `type`.

A justificativa para a utilização de exportação nomeada em vez de exportação padrão (export default) em hooks, contexts, serviços ou dependências está relacionada à clareza e manutenção do código. Ao adotar a exportação nomeada, você explicitamente nomeia as funções ou classes que está exportando, o que torna mais fácil entender quais recursos estão disponíveis e como devem ser utilizados.

### Pasta de componentes

**Todo componente**, independentemente do tipo e escopo, deve ser organizado em pastas e não como arquivos soltos. Essa prática favorece uma maior coesão entre os componentes e suas dependências, como estilos. A estrutura de arquivo padrão segue o formato abaixo:

```sh
context/
|-- customComponent/
|   |-- index.tsx # Re-export
|   |-- customComponent.tsx # Código contendo o componente
|   |-- customComponent.styles.tsx # (opcional) Arquivo com estilos
|   |-- customComponent.config.tsx # (opcional) Arquivo com configuração
```

Conforme mencionado anteriormente, o padrão de exportação utilizado é o nomeado. Isso é aplicável a todos os arquivos, exceto ao `index.tsx`. Neste arquivo, realizamos a reexportação do nosso componente utilizando o formato **default**.

```jsx title="customComponent/index.tsx"
export { customComponent as default } from "./customComponent";
```

Essa estrutura proporciona uma organização mais clara e modular, facilitando a manutenção e compreensão do código. Ao agrupar componentes relacionados em pastas dedicadas, torna-se mais simples gerenciar seus arquivos associados, como estilos e configurações. Adotar um arquivo único para o componente, em vez de criar diretamente no index, evita ter vários arquivos com o mesmo nome abertos de uma só vez no editor de códigos. Além disso, facilita a localização de arquivos utilizando atalhos de busca.

### Arquivo de componente

O arquivo principal de um componente deve sempre conter as definições de suas propriedades (tipo Props) e sua função principal. O export deve ser nomeado, e o componente deve residir em um arquivo com seu próprio nome. **Evite** colocar um componente diretamente no arquivo `index.tsx`.

```jsx
import type { FC } from "react";
import { View, Text } from "react-native";

interface Props {}

const CustomComponent01: FC<Props> = () => {
  return (/* Componente JSX aqui */);
}

export { CustomComponent01 }
```

Também é uma boa prática exportar explicitamente o tipo das propriedades, apesar de a inferência de tipos poder ser feita posteriormente. Neste caso, nomeamos as propriedades de forma mais específica.

```jsx
import type { FC } from "react";
import { View, Text } from "react-native";

interface CustomComponent01Props {}

const CustomComponent01: FC<CustomComponent01Props> = () => {
  return (/* Componente JSX aqui */);
}

export { CustomComponent01 }
export type { CustomComponent01Props }
```

> Por padrão, todas as interfaces de propriedade do React devem ter o mesmo nome do seu componente seguido pela palavra "Props" no final.

### Compound

**Componentes compostos** ou "compound components" não representam um único componente, mas sim um conjunto de vários componentes construídos de maneira que devem sempre atuar em conjunto. Essa abordagem encapsula tanto o estado quanto o comportamento de um grupo de componentes, permitindo que o usuário externo tenha controle sobre a renderização das partes variáveis.

O exemplo a seguir é implementado em Vue, utilizando a biblioteca HeadlessUI, para criar um menu composto por vários componentes especializados.

```vue
<template>
  <Menu>
    <MenuButton>More</MenuButton>
    <MenuItems>
      <MenuItem v-slot="{ active }">
        <a :class="{ 'bg-blue-500': active }" href="/account-settings"> Account settings </a>
      </MenuItem>
      <MenuItem v-slot="{ active }">
        <a :class="{ 'bg-blue-500': active }" href="/account-settings"> Documentation </a>
      </MenuItem>
      <MenuItem disabled>
        <span class="opacity-75">Invite a friend (coming soon!)</span>
      </MenuItem>
    </MenuItems>
  </Menu>
</template>

<script setup>
import { Menu, MenuButton, MenuItems, MenuItem } from "@headlessui/vue";
</script>
```

A versão React do mesmo exemplo, utilizando a biblioteca HeadlessUI, é apresentada abaixo:

```jsx
import { Menu } from "@headlessui/react";

function MyDropdown() {
  return (
    <Menu>
      <Menu.Button>More</Menu.Button>
      <Menu.Items>
        <Menu.Item>
          {({ active }) => (
            <a className={`${active && "bg-blue-500"}`} href="/account-settings">
              Account settings
            </a>
          )}
        </Menu.Item>
        <Menu.Item>
          {({ active }) => (
            <a className={`${active && "bg-blue-500"}`} href="/account-settings">
              Documentation
            </a>
          )}
        </Menu.Item>
        <Menu.Item disabled>
          <span className="opacity-75">Invite a friend (coming soon!)</span>
        </Menu.Item>
      </Menu.Items>
    </Menu>
  );
}
```

Ambos exemplos apresentam um menu composto por vários componentes especializados. No Vue, há vários desses componentes, enquanto no React, há apenas um componente `Menu`, contendo todos os elementos necessários para lidar com botões, itens e lógica do sistema.

Ainda assim, podemos dizer que não há uma abordagem certa ou errada; ambas funcionam dentro de suas propostas. No entanto, dada a estrutura criada, utilizando contextos separados dos componentes, o exemplo Vue se torna mais apropriado.

Ao aplicarmos essa abordagem ao React, teríamos algo semelhante ao que é feito pela biblioteca Material-UI (`Mui`).

```jsx
import * as React from 'react';
import Button from '@mui/material/Button';
import Menu from '@mui/material/Menu';
import MenuItem from '@mui/material/MenuItem';

export default function BasicMenu() {
  const [anchorEl, setAnchorEl] = React.useState<null | HTMLElement>(null);
  const open = Boolean(anchorEl);
  const handleClick = (event: React.MouseEvent<HTMLButtonElement>) => {
    setAnchorEl(event.currentTarget);
  };
  const handleClose = () => {
    setAnchorEl(null);
  };

  return (
    <div>
      <Button
        id="basic-button"
        aria-controls={open ? 'basic-menu' : undefined}
        aria-haspopup="true"
        aria-expanded={open ? 'true' : undefined}
        onClick={handleClick}
      >
        Dashboard
      </Button>
      <Menu
        id="basic-menu"
        anchorEl={anchorEl}
        open={open}
        onClose={handleClose}
        MenuListProps={{
          'aria-labelledby': 'basic-button',
        }}
      >
        <MenuItem onClick={handleClose}>Profile</MenuItem>
        <MenuItem onClick={handleClose}>My account</MenuItem>
        <MenuItem onClick={handleClose}>Logout</MenuItem>
      </Menu>
    </div>
  );
}
```

Comparando a estrutura de composto mais tradicional do React com a estrutura do `Mui`, percebemos que a segunda é um pouco mais complexa de desenvolver, envolvendo um maior número de pastas e arquivos. No entanto, a organização proporciona um maior controle individual de cada elemento.

A estrutura mais comum trata todos os componentes como uma única unidade, gerenciando o `context` no próprio `index.tsx`. A existência de um componente `Root` é opcional, e o index pode ser o componente raiz, mas o uso do TypeScript pode gerar problemas na declaração das propriedades.

```sh title="Estrutura de compound - Flat"
components/
|-- menu/
|   |-- index.tsx            # Re-export
|   |-- Buttom.tsx           # Código contendo o componente Button
|   |-- Item.tsx             # Código contendo o componente Item
|   |-- Items.tsx            # Código contendo o componente Items
|   |-- Root.tsx             # Código contendo o componente Root
|   |-- Buttom.styles.tsx    # Código contendo os estilos para o componente Button
|   |-- Item.styles.tsx      # Código contendo os estilos para o componente Item
|   |-- Items.styles.tsx     # Código contendo os estilos para o componente Items
|   |-- Root.styles.tsx      # Código contendo os estilos para o componente Root
```

Além da estrutura flat, é possível adotar uma estrutura aninhada, particularmente útil quando há muitos estilos ou arquivos de configuração.

```sh title="Estrutura de compound - Nested"
components/
|-- menu/
|   |-- index.tsx             # Re-export
|   |-- Button/
|   |   |-- index.tsx         # Re-export
|   |   |-- Button.tsx        # Código contendo o componente Button
|   |   |-- Button.styles.tsx # Código contendo os estilos para o componente Button
|   |
|   |-- Item/
|   |   |-- index.tsx         # Re-export
|   |   |-- Item.tsx          # Código contendo o componente Item
|   |   |-- Item.styles.tsx   # Código contendo os estilos para o componente Item
|   |
|   |-- Items/
|   |   |-- index.tsx         # Re-export
|   |   |-- Items.tsx         # Código contendo o componente Items
|   |   |-- Items.styles.tsx  # Código contendo os estilos para o componente Items
|   |
|   |-- Root/
|       |-- index.tsx         # Re-export
|       |-- Root.tsx          # Código contendo o componente Root
|       |-- Root.styles.tsx   # Código contendo os estilos para o componente Root
```

Ao mesclarmos o estilo de código da `Mui` a organização proporcionada pelo sistema de `features`, temos a seguinte forma:

```sh title="Estrutura de compound - Adaptada"
shared/
|-- components/                   # Re-export
|   |-- Menu/
|   |   |-- index.tsx                 # Re-export
|   |   |-- Menu.tsx                  # Código contendo o componente Menu
|   |   |-- Menu.styles.tsx           # Código contendo os estilos para o componente Menu
|   |
|   |-- MenuButton/
|   |   |-- index.tsx                 # Re-export
|   |   |-- MenuButton.tsx            # Código contendo o componente MenuButton
|   |   |-- MenuButton.styles.tsx     # Código contendo os estilos para o componente MenuButton
|   |
|   |-- MenuItem/
|   |   |-- index.tsx                 # Re-export
|   |   |-- MenuItem.tsx              # Código contendo o componente MenuItem
|   |   |-- MenuItem.styles.tsx       # Código contendo os estilos para o componente MenuItem
|   |
|   |-- MenuItems/
|       |-- index.tsx                 # Re-export
|       |-- MenuItems.tsx             # Código contendo o componente MenuItems
|       |-- MenuItems.styles.tsx      # Código contendo os estilos para o componente MenuItems
|
|-- context/
    |-- menuContext/                  # Contexto relacionado ao Menu
        |-- index.tsx                 # Re-export
        |-- menuContext.context.tsx   # Código contendo a definição do contexto
        |-- menuContext.hook.tsx      # Código contendo o hook relacionado ao contexto
        |-- menuContext.provider.tsx  # Código contendo o provedor do contexto
```

Essa estrutura proporciona uma organização mais clara e modular, facilitando a manutenção e compreensão do código. Apesar de um pouco mais complexa, oferece um maior controle sobre os elementos individuais.