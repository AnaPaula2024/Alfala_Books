## Conectando o Front ao Back: Guia Completo

Este guia detalhado irá guiá-lo através do processo de conexão do seu frontend React com o backend, utilizando Axios para requisições HTTP e configurando as rotas para uma navegação fluida.

**1. Instalando Dependências**

No diretório `frontend`, instale o `react-router-dom` para gerenciamento de rotas:

```bash
npm install react-router-dom
```

**2. Configurando Rotas no `index.js`**

Importe os componentes necessários do `react-router-dom`:

```javascript
import { BrowserRouter, Routes, Route } from 'react-router-dom';
```

Envolva sua aplicação com o `BrowserRouter` e defina as rotas dentro do `Routes`:

```javascript
import React from 'react';
import ReactDOM from 'react-dom/client';
import App from './App';
import reportWebVitals from './reportWebVitals';
import { createGlobalStyle } from 'styled-components';
import { BrowserRouter, Routes, Route } from "react-router-dom";

// ... (código omitido)

const root = ReactDOM.createRoot(document.getElementById('root'));
root.render(
  <React.StrictMode>
    <GlobalStyle />
    <BrowserRouter>
      <Routes>
        <Route path="/favoritos" element={<p>Oi!</p>} />
        <Route path="/" element={<App />} />
      </Routes>
    </BrowserRouter>
  </React.StrictMode>
);
```

**Componentes:**

*   **BrowserRouter**: Fornece o contexto de roteamento para a aplicação.
*   **Routes**: Define o conjunto de rotas.
*   **Route**: Define uma rota individual com `path` (URL) e `element` (componente a ser renderizado).

**3. Criando Rotas para a Aplicação**

**3.1. Adicionando Links ao Cabeçalho (`OptionsHeader/index.js`)**

Importe o `Link` do `react-router-dom` e envolva as opções de navegação com a tag `<Link>`:

```javascript
import styled from 'styled-components';
import { Link } from 'react-router-dom';

// ... (código omitido)

function OptionsHeader() {
  return (
    <Opcoes>
      {textoOpcoes.map((texto) => (
        <Link to={`/${texto.toLowerCase()}`} key={texto}>
          <Opcao><p>{texto}</p></Opcao>
        </Link>
      ))}
    </Opcoes>
  );
}

export default OptionsHeader;
```

**3.2. Ajustando Rotas no `index.js`**

Importe o componente `Header` e ajuste as rotas:

```javascript
import { BrowserRouter, Routes, Route } from 'react-router-dom';
import Header from './components/Header';
// ... (código omitido)
import Home from './routes/Home';
import Favorites from './routes/Favorites';

// ... (código omitido)

root.render(
  <React.StrictMode>
    <GlobalStyle />
    <BrowserRouter>
      <Header />
      <Routes>
        <Route path="/favoritos" element={<Favorites />} />
        <Route path="/" element={<Home />} />
      </Routes>
    </BrowserRouter>
  </React.StrictMode>
);
```

**4. Criando a Página de Favoritos (`routes/Favorites.js`)**

Crie o arquivo `routes/Favorites.js` e adicione o conteúdo da página:

```javascript
import styled from 'styled-components';
// ... (outros imports)

// ... (estilos)

function Favorites() {
  // ... (lógica da página)
}

export default Favorites;
```

**5. Link no Logo (`components/Header/index.js`)**

Adicione um `Link` ao redor do logo para redirecionar para a página inicial:

```javascript
import { Link } from "react-router-dom";

// ... (código omitido)

function Header() {
  return (
    <HeaderContainer>
      <Link to="/">
        <Logo/>
      </Link>
      {/* ... */}
    </HeaderContainer>
  );
}
```

**6. Renomeando `App.js` para `Home.js`**

Renomeie o arquivo `App.js` para `Home.js` e ajuste a função:

```javascript
// Home.js
function Home() {
  return (
    <AppContainer>
      <Pesquisa />
      <Search />
      <LatestReleases />
    </AppContainer>
  );
}

export default Home;
```

**7. Movendo `Home.js` para `routes`**

Mova o arquivo `Home.js` para a pasta `routes`.

**8. Duplicando `Home.js` para `Favorites.js`**

Duplique o arquivo `Home.js` e renomeie para `Favorites.js`.

**9. Ajustando `index.js`**

*   Remova o import `App from './App';`
*   Insira `import Home from './routes/Home';`
*   Altere `<Route path="/" element={<App />} />` para `<Route path="/" element={<Home />} />`

**10. Ajustando `Favoritos.js`**

Remova componentes e imports não utilizados.

**11. Instalando Axios**

```bash
npm install axios
```

**12. Criando `services/livro.js`**

```javascript
import axios from "axios";

const livrosAPI = axios.create({ baseURL: "http://localhost:8000/livros" });

async function getLivros() {
  const response = await livrosAPI.get('/');
  return response.data;
}

export { getLivros };
```

**13. Ajustando `components/Pesquisa/index.js`**

*   Remova `import { livros } from "./dadosPesquisa";`
*   Remova `console.log(livrosPesquisados)`
*   Adicione:

```javascript
import { useEffect, useState } from "react";
import { getLivros } from '../../service/livros.js';

// ... (código omitido)

function Pesquisa() {
  const [livrosPesquisados, setLivrosPesquisados] = useState([]);
  const [livros, setLivros] = useState([]);

  useEffect(() => {
    async function fetchLivros() {
      const livrosDaAPI = await getLivros();
      setLivros(livrosDaAPI);
    }
    fetchLivros();
  }, []);

  // ... (código omitido)
}
```

**14. Ajustando `services/livro.js` (Novamente)**

```javascript
import axios from "axios";

const livrosAPI = axios.create({ baseURL: "http://localhost:8000/livros" });

async function getLivros() {
  const response = await livrosAPI.get('/');
  return response.data;
}

export { getLivros };
```

**15. Verificando Logs**

Inspecione o console do navegador para verificar erros de CORS.

**16. Instalando CORS no Backend**

```bash
npm install cors
```

**17. Ajustando `app.js` (Backend)**

```javascript
// ... (outros imports)
import cors from 'cors';

const app = express();

app.use(express.json());
app.use(cors({ origin: "*" }));

// ... (resto do código)
```

**18. Criando `favoritos.json` e `livros.json` (Backend)**

Crie os arquivos `favoritos.json` e `livros.json` com dados de exemplo.

**19. Criando `services/favoritosServices.js` (Backend)**

```javascript
import fs from 'fs';

function getTodosFavoritos() {
  return JSON.parse(fs.readFileSync("favoritos.json"));
}

function deletaFavoritoPorId(id) {
  const livros = JSON.parse(fs.readFileSync("favoritos.json"));
  const livrosFiltrados = livros.filter(livro => livro.id !== id);
  fs.writeFileSync("favoritos.json", JSON.stringify(livrosFiltrados));
}

function insereFavorito(id) {
  const livros = JSON.parse(fs.readFileSync("livros.json"));
  const favoritos = JSON.parse(fs.readFileSync("favoritos.json"));

  const livroInserido = livros.find(livro => livro.id === id);
  const novaListaDeLivrosFavoritos = [...favoritos, livroInserido];
  fs.writeFileSync("favoritos.json", JSON.stringify(novaListaDeLivrosFavoritos));
}

export { getTodosFavoritos, deletaFavoritoPorId
