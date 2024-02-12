## Sistema de Features

### Definição

Ao discutir o conceito de features, estamos nos referindo a um sistema de módulos projetados especificamente para a camada de apresentação de um aplicativo. Esses módulos desempenham um papel crucial ao isolar componentes e funcionalidades, simplificando o processo de codificação e reduzindo os efeitos colaterais decorrentes de possíveis alterações. Essa abordagem é particularmente valiosa ao lidar com projetos complexos que abrangem diversas funcionalidades distintas.

```sh title="Estrutura das features"
src/
|-- features/
|   |-- travel/
|       |-- components/     // Componentes como buttons, modals, etc.
|       |-- utils/          // Funções utilitários
|       |-- hooks/          // React hooks
|       |-- styles/         // Estilos e temas
|   |-- hotel/
|       |-- components/     // Componentes como buttons, modals, etc.
|       |-- utils/          // Funções utilitários
|       |-- hooks/          // React hooks
|       |-- styles/         // Estilos e temas
|-- views/
|-- app.js
```

### Utilização

A utilização de features permite a separação de recursos locais em módulos independentes. No entanto, isso não implica na necessidade de criar uma feature para cada pequeno escopo da aplicação. Features são usualmente criadas para funcionalidades mais robustas, como autenticação e comunicação. Conteúdos especificos como navegação, sócios, viagens e hoteis também possuem, ou devem possuir, suas próprias features.

```sh title="Exemplos"
src/
|-- features/
|   |-- auth/           // Autenticação
|   |-- customers/      // Gestão de clientes
|   |-- posts/          // Publicações e artigos
|-- views/
|-- app.js
```

Além de ponderar o que deve ser uma feature, também é necessário ter em mente qual deve ser o seu conteúdo. Em geral, queremos ter apenas lógicas de apresentação (ou seja, que fazem parte de componentes). Alguns exemplos:

- `/components`: Principal pasta de uma feature, contendo elementos reutilizáveis e específicos do contexto.
- `/compounds`: Inclui componentes complexos que, apesar de sua modularidade e potencial de reutilização global, podem ser especializados por feature.
- `/layouts`: Focados na estruturação visual e espacial dos componentes, sem envolver-se diretamente com a lógica de negócios.
- `/hooks`: Custom hooks que encapsulam lógicas de estado ou efeitos secundários específicos da feature.

A adoção de features constituiu em uma escolha de design orientada a atenuar questões estruturais e estéticas originadas por outras formas de organização. Assim como essas alternativas, a abordagem de features apresenta tanto vantagens quanto desvantagens.

### Vantagens

- **Isolamento Lógico**: As features proporcionam uma clara divisão lógica no código, permitindo a isolação de funcionalidades específicas. Isso facilita a compreensão e manutenção do sistema.

- **Reusabilidade**: A modularidade inerente ao sistema de features promove a reusabilidade de código, permitindo que módulos inteiros sejam facilmente incorporados em diferentes partes do aplicativo.

- **Manutenção**: Alterações e atualizações podem ser aplicadas a uma feature sem afetar outras partes do sistema, simplificando a manutenção e reduzindo o risco de regressões.

- **Escalabilidade**: Adicionar novas funcionalidades torna-se mais fácil, pois as features podem ser desenvolvidas e integradas independentemente umas das outras.

### Desvantagens

- **Complexidade**: Em projetos pequenos, a introdução de um sistema de features pode parecer excessiva, adicionando complexidade desnecessária à estrutura inicial.

- **Possível Duplicação de Lógica**: Se não gerenciada adequadamente, a modularização por features pode resultar na duplicação de lógica entre diferentes módulos.

### Comparações

- **Pasta de componentes**: A organização de todos os componentes em uma única pasta resulta em uma poluição visual significativa à medida que a aplicação cresce. Isso aumenta o tempo necessário para localizar funcionalidades específicas e promove o acoplamento. A proximidade física dos componentes facilita o uso indiscriminado uns nos outros.

- **Componentes com views**: Apesar da facilidade inicial de estruturação e localização de dependências, a prática de colocar componentes na pasta de views/pages/screens pode levar a um alto grau de acoplamento e desorganização à medida que o projeto cresce. A reusabilidade é prejudicada quando um componente específico de uma view é necessário em outra, e o desenvolvimento vertical pode ser prejudicial para documentação e testes.

## Features globais

### Definição

Enquanto as features se ocupam exclusivamente de escopos locais e específicos, a estrutura do projeto reserva a pasta `shared` para lidar com componentes e hooks de escopo global.

```sh
src/
|-- shared/
|   |-- components/     // Componentes como buttons, modals, etc.
|   |-- utils/          // Funções utilitários
|   |-- hooks/          // React hooks
|   |-- styles/         // Estilos e temas
|-- views/
|-- app.js
```

Embora seja teoricamente possível agrupar o conteúdo da `shared` dentro de uma feature denominada `common`, decidimos mantê-la na pasta raiz por motivos de clareza e acessibilidade. Esta escolha visa facilitar a identificação e utilização de recursos globais, reforçando a coesão e a modularidade do código.

### Justificativa

Contrariamente à prática comum de agrupar elementos globais dentro da pasta `shared`, com exceção de certos elementos, como services, adotamos aqui uma abordagem distinta: **apenas o que é permitido a uma feature pode residir dentro da pasta shared.** Isso significa que a `shared` nada mais é que uma `feature`.

A definição pode parecer um tanto intricada à primeira vista, mas o que queremos expressar é que, semelhante a uma feature, a pasta shared deve conter apenas entidades pertencentes à camada de apresentação. A inclusão de um objeto na shared é permitida apenas se esse objeto contribuir diretamente para a construção de telas e componentes.

Essa restrição se faz essencial para estabelecer um "espelhamento" entre as features, promovendo uma coesão organizacional mais consistente. Tal coerência revela-se crucial ao enfrentar o desafio de compreender o sistema, especialmente durante os estágios iniciais de familiarização com a estrutura da aplicação.

## Services

### Definição

No contexto do desenvolvimento de software, o termo "serviços" refere-se a módulos ou classes dedicados a lidar com a lógica de negócios que não está intrinsecamente vinculada à camada de apresentação (UI). Em termos simples, qualquer lógica que não esteja diretamente relacionada à exibição na tela pode ser encapsulada em um serviço.

```js title="Exemplo de serviço"
const apiService = {
  fetchData: async (endpoint) => {
    // Lógica para fazer chamadas à API
    // ...
    return fetchedData;
  },de u
  // Outras operações relacionadas à API
};

export default apiService;
```

É crucial observar que, mesmo que o serviço seja consumido por um componente, ele não é parte integrante do componente em si. O serviço, como entidade separada, tem a responsabilidade exclusiva de fornecer informações para serem exibidas na tela, sem exercer influência direta sobre a funcionalidade do componente. Normalmente, a conexão entre a camada de apresentação e a camada de aplicação no React é estabelecida por meio do hook `useEffect`.

```jsx title="Exemplo de consumo de serviço"
import { useState, useEffect } from "react";
import apiService from "./caminho/do/seu/apiService";

const ExemploComponente = () => {
  const [data, setData] = useState(null);

  useEffect(() => {
    const fetchData = async () => {
      const result = await apiService.fetchData("/endpoint");
      setData(result);
    };

    fetchData();
  }, []);

  return (
    <div>
      {data && (
        <div>
          <h1>Dados da API</h1>
          <pre>{JSON.stringify(data, null, 2)}</pre>
        </div>
      )}
    </div>
  );
};

export default ExemploComponente;
```

Em React chamamos isso de side effect, ou efeito colateral. Chamadas à API são assíncronas. Se você realizar a chamada diretamente no corpo do componente, pode enfrentar problemas relacionados à sincronicidade, onde os dados podem não estar disponíveis no momento da renderização.

### Utilização

No ecossistema React, a prática comum envolve o uso de serviços como funções ou objetos instanciados. Porém isso pode causar problemas relacionados à testabilidade e performance. Optamos por adotar uma abordagem inspirada no conceito de injeção de dependência, assemelhando-se à prática do Angular. No entanto, devido a limitações da biblioteca React, essa injeção de dependência foi implementada diretamente no arquivo `index.ts`.

> É válido observar que é possível criar um sistema de injeção de dependência automatizado no React. No entanto, essa implementação demandaria a utilização de bibliotecas de terceiros e configurações complexas.

```ts title="Amostra parcial de serviço presente no sistema"
// src/services/ApiService.ts
class ApiService {
  private _bearerToken: string = "";

  constructor(private readonly axios: AxiosInstance) {}

  set bearerToken(token: string) {
    this._bearerToken = token;
    this.axios.defaults.headers["Authorization"] = `Bearer ${token}`;
  }

  get bearerToken(): string {
    return this._bearerToken;
  }

  async get<T, U>(url: string, params?: T): Promise<U> {
    try {
      const response = await this.axios.get<U>(url, { params });
      return response.data;
    } catch (e) {
      console.warn("🚀 ~ file: ApiService.ts:15 ~ ApiService ~ e:", e);
      throw e;
    }
  }
}
```

```ts title="Instância do serviço"
// src/services/index.ts
import axios from "axios";
import { ApiService } from "./ApiService";

const axiosInstance = axios.create({
  baseURL: process.env.API_URL,
});

const apiService = new ApiService(axiosInstance);

export { apiService };
```

Essa implementação exemplifica a criação de um serviço de API (ApiService) que encapsula chamadas à API utilizando a biblioteca Axios. O arquivo index.ts atua como ponto central para a criação e exportação do serviço, estabelecendo um padrão que permite a injeção consistente de dependências em todo o aplicativo React.

### Testes

Embora testar a camada de apresentação seja um desafio relativamente complexo, os testes, sejam unitários ou de integração, na camada de aplicação são menos complicados e dispensam a necessidade de renderizar componentes ou criar um servidor. Ao empregar injeção de dependência e a biblioteca axios-mock-adapter, nossa tarefa se torna consideravelmente mais fácil.

```ts
// src/services/__tests__/ApiService.spec.ts
import axios, { AxiosInstance } from "axios";
import MockAdapter from "axios-mock-adapter";
import { ApiService } from "../ApiService";

describe("ApiService", () => {
  let apiService: ApiService;
  let mock: MockAdapter;

  beforeEach(() => {
    const axiosInstance = axios.create();

    mock = new MockAdapter(axiosInstance);
    apiService = new ApiService(axiosInstance);
  });

  afterEach(() => {
    mock.reset();
  });

  describe(".get(url, params?)", () => {
    it("deve realizar uma chamada à API corretamente", async () => {
      const mockData = { id: 1, title: "Mock Title" };
      mock.onGet("/example").reply(200, mockData);

      const result = await apiService.get("/example");

      expect(result).toEqual(mockData);
    });

    it("deve lidar com erros na chamada à API", async () => {
      const mockData = { error: "Internal Server Error" };
      mock.onGet("/error").reply(500, mockData);

      const resultPromise = apiService.get("/error");

      expect(resultPromise).rejects.toThrow(AxiosError);
    });
  });
});
```

Neste exemplo, adotamos um padrão para testes de classes em que o primeiro bloco describe contém o nome da classe, e o segundo bloco describe contém o nome e os parâmetros da função que está sendo testada. Essa abordagem aprimora a legibilidade dos logs durante a execução dos testes. Além disso, é crucial testar não apenas os casos "felizes", mas também possíveis cenários de erro para garantir a robustez do serviço.

## Forms

### Definição

Em geral, as funções de validação garantem que os dados inseridos nos formulários atendam aos requisitos necessários. Elas podem incluir validações de formato, comprimento, presença obrigatória e outras regras específicas.

No caso do React Native, esse efeito é ainda mais pronunciado, uma vez que, além de lidar com as restrições de dados, também é necessário superar as limitações da própria entrada de dados, que possui tipos e formatos menos abrangentes que sua versão web. Para isso, utilizamos as seguintes bibliotecas: react-hook-form, zod e react-native-masked-text.

### Máscara de input

Para adicionar mascáras foi escolhida a biblioteca `react-native-masked-text` a qual, junto com o componente nativo `TextInput` foi utilizado para a criação de um uníco componente de entrada de dados personalizável.

```jsx
if (mask) return <TextInputMask type={mask} options={maskOptions} {...rest} />;

return <TextInput {...rest} />;
```

Infelizmente, componentes de entrada customizados tendem a causar problemas com a validação mais comum utilizada pelo `react-hook-form`. Isso implica que, para evitar problemas com a implementação atual e fututuras, é necessariam uma abordagem alternativa, porém igualmente válida, utilizando controllers.

### Inputs registrados

Ao utilizar o `react-hook-form` a maneira mais comum de fazer o biding de um componente, ou seja, vincular ele a biblioteca é utilizando a função `register`.

```jsx title="React Hook Form - Register Fields"
export default function App() {
  const { register, handleSubmit } = useForm();

  return (
    <form onSubmit={handleSubmit((data) => console.log(data))}>
      <label>First Name</label>
      <input {...register("firstName")} />
      <input type="submit" />
    </form>
  );
}
```

É uma abordagem simples, onde o "firstName" se torna o ID do objeto, enquanto a função retorna objetos como value e onChange os quais lidam com a mudança de valores internos dentro do componente. Infelizmente, isso não funciona corretamente no nosso caso.

### Inputs controlados

Entradas controladas possibilitam um desenvolvimento mais refinado ao registrar componentes. Isso é especialmente útil para lidar com itens personalizados ou simplesmente incompatíveis. Existem duas principais maneiras de utilizar um `control`: a primeira envolve o componente `Controller`, enquanto a segunda faz uso do hook `useController`.

```jsx title="Exemplo utilizando Controller"
import { Controller, useForm } from "react-hook-form";

function MeuFormulario() {
  const { control, handleSubmit } = useForm();

  return (
    <form onSubmit={handleSubmit((data) => console.log(data))}>
      <Controller
        name="exemploCampo"
        control={control}
        defaultValue=""
        render={({ field }) => <input {...field} />}
      />
      <button type="submit">Enviar</button>
    </form>
  );
}
```

```jsx title="Exemplo utilizando useController"
import { Controller, useForm } from "react-hook-form";

function MeuFormulario() {
  const { control, handleSubmit } = useForm();

  const { field } = useController({
    name: "exemploCampo",
    control,
    defaultValue: "",
  });

  return (
    <form onSubmit={handleSubmit((data) => console.log(data))}>
      <input {...field} />
      <button type="submit">Enviar</button>
    </form>
  );
}
```

À primeira vista, o uso do componente `Controller` parece ser mais simples do que o hook `useController`. No entanto, à medida que o componente cresce em complexidade, a gestão de vários valores por meio de um único componente torna-se complicada, e nesse cenário, o hook acaba se destacando como a melhor opção.

Infelizmente, essa percepção demorou um pouco para se manifestar durante o desenvolvimento da aplicação. Inicialmente, afetava apenas áreas específicas. Contudo, à medida que o uso e a integração de formulários aumentaram, tornou-se imperativo criar soluções internas mais elaboradas para facilitar a reutilização. Como resultado desse processo, optamos por **migrar do uso de componente para hook**.

### Schemas

Enquanto o `react-hook-form` possui um sistema próprio de validação de entrada de dados ele é bem limitado em certos aspectos. Para superar essa limitação, utilizamos bibliotecas extenas de validão como `yup` e `zod` que extendem essas funcionalidades. Sendo assim, criamos `schemas`.

Em um contexto de formulários e validações, os schemas referem-se a estruturas ou descrições que definem as regras e a forma que os dados devem ter para serem considerados válidos. Nos sistemas de formulários, os schemas são comumente usados para garantir que os dados inseridos atendam aos requisitos específicos estabelecidos pelas aplicações.

```js
import { z } from "zod";

const schema = z.object({
  firstName: z.string().min(2).max(50),
  lastName: z.string().min(2).max(50),
  email: z.string().email(),
});
```

No contexto do React Native o `zod` tem um papel maior do que simplesmente validar campos: Ele é amplemente utilizado para fazer a conversão de tipo entre strings e outros primitivos.

Ao contrário do React ou mesmo do HTML, o React Native possui uma quantidade bastante limitada de opções para entrada de dados. Embora muitos tipos de inputs possam ser substituídos por bibliotecas externas, o `zod` pode ser empregado para assegurar que, independentemente da forma como os dados são inseridos, o formulário seja sempre enviado com os tipos corretos para o servidor.

```js
import { z } from "zod";

const schema = object({
  username: string(),
  age: string().refine((age) => !isNaN(parseInt(age)), {
    message: "Idade deve ser um número.",
  }),
});
```

Finalmente, a terceira vantagem, que motivou a escolha dele em detrimento do `yup`, é a integração com TypeScript. A partir dos esquemas (schemas), podemos criar tipos utilizando a propriedade `infer`.

```js
const stringToNumber = z.string().transform((val) => val.length);
type inferred = z.infer<typeof stringToNumber>; // number
```

> É importante notar que, atualmente, o `yup` possui funcionalidades semelhantes, no entanto, seu suporte ao TypeScript é mais recente.

## Navigation

### Definição

Atualmente, a versão mais recentes do Expo possui seu próprio sistema de navegação baseado em filesystem, porém, quando este projeto foi criado, apesar de estável ainda era extremamente recente. Graças a isso, foi escolhida a biblioteca `react-native-navigation`, a qual é amplamente usada e testada dentro do ecossistema React Native.

### Arquivos

Enquanto não existe uma necessidade real em dispor a navegação de projetos pequenos em diversos arquivos, uma vez que o navegador é mais um componente react, isso muda quanto possuimos tantas telas, escopos e tipos diferentes de navegadores quanto há atualmente no projeto. Para isso, a estrutura de navagação foi divida em três escopos: App, Auth e Hotel. Os quais são dividos em vários arquivos de configurações para facilitar a reutilização de opções e tipos. Sendo eles listados a seguir.

- module.screens.tsx
- module.config.tsx
- module.navigator.tsx
- module.params.tsx

### Screens

O arquivo `modulo.screen.tsx` representa todas as telas presentes naquele módulo de navegação especifico. Agrupas todas em um único lugar ajuda a manter uma lista coerente de telas, tornando simples adicionar e remover elas.

```ts title="navigation/hotel/hotel.screens.ts"
import HotelScreen from "@/screens/HotelScreen";
import HotelDetailsScreen from "@/screens/HotelDetailsScreen ";

const hotelScreens = {
  HotelScreen,
  HotelDetailsScreen,
};

export { hotelScreens };
```

### Config

O arquivo `module.config.tsx` possui as configurações global e locais das telas e grupos de telas, assim como a tipagem necessária para garantir que sempre exista a configuração correta baseada nas telas existentes.

```ts title="navigation/hotel/hotel.config.ts"
import type { NativeStackNavigationOptions } from "@react-navigation/native-stack";
import { hotelScreens } from "./hotel.screens";
import { theme } from "@/shared/styles/theme";
import HeaderBackButton from "@/features/nav/components/HeaderBackButton";

type Screens = "Default" | keyof typeof hotelScreens;
type Options =
  | NativeStackNavigationOptions
  | (({ navigation }: any) => NativeStackNavigationOptions);

const hotelConfig: Record<Screens, Options> = {
  Default: ({ navigation }) => ({
    headerStyle: {
      backgroundColor: theme.colors.primary,
    },
    headerTintColor: "#fff",
    headerLeft: () => <HeaderBackButton mr={4} navigation={navigation} />,
  }),

  HotelScreen: {},

  HotelDetailsScreen: {
    headerTransparent: true,
    headerStyle: {
      backgroundColor: theme.colors.transparent,
    },
    headerTitle: "",
  },
};

export { hotelConfig };
```

> O tipo Options é um dos poucos lugares em todo código que possui algum "any". Isso ocorre devido a falta de tipagem apropriada da própria biblioteca. Sempre que possível use "unkwown" ou "never", nunca "any".

### Params

O arquivo `module.params.tsx` contém os parametros que são passados para as telas, tais como ID, titúlos, e etc.

```ts title="navigation/hotel/hotel.params.ts"
import type { ParamListBase } from "@react-navigation/native";

interface HotelParams extends ParamListBase {
  HotelDetailsScreen: {
    id: string;
  };
}

export type { HotelParams };
```

Não é necessário declarar cada tela existente, apenas as que recebem algum argumento de fato. O tipo `ParamListBase` se encarrega de lidar com quaisquer telas que não são corretamente declaradas.

### Navigator

O arquivo `module.navigator.tsx` faz a instancia de todas as configurações necessárias para a criação das rotas.

```jsx title="navigation/hotel/hotel.navigator.ts"
import type { HotelParams } from "./hotel.params";
import { createNativeStackNavigator } from "@react-navigation/native-stack";
import { hotelConfig } from "./hotel.config";
import { hotelScreens } from "./hotel.screens";

const Stack = createNativeStackNavigator<HotelParams>();

const HotelNavigator = () => {
  return (
    <Stack.Navigator initialRouteName="HotelScreen" screenOptions={hotelConfig.Default}>
      <Stack.Screen
        name={hotelScreens.HotelScreen.name}
        component={hotelScreens.HotelScreen}
        options={hotelConfig.HotelScreen}
      />

      <Stack.Screen
        name={hotelScreens.HotelDetailsScreen.name}
        component={hotelScreens.HotelDetailsScreen}
        options={hotelConfig.HotelDetailsScreen}
      />
    </Stack.Navigator>
  );
};

export { HotelNavigator };
```