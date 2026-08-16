const CANAIS = [
  {
    id: "canal-sete",
    nome: "Canal Sete",
    categoria: "Geral",
    stream: "http://play.canalsete.com.br:1935/canalsete/xpl-c7_source/playlist.m3u8",
    imagem: "https://i.imgur.com/0cq0BPp.png"
  },
  {
    id: "canal-uol",
    nome: "Canal UOL",
    categoria: "Geral",
    stream: "https://video24.mais.uol.com.br/live/6146.m3u8",
    imagem: ""
  },
  {
    id: "tv-cultura-pa",
    nome: "TV Cultura PA",
    categoria: "TV Aberta",
    stream: "http://177.74.1.38/funtelpa/tv_funtelpa/playlist.m3u8",
    imagem: "http://s20.postimg.org/m7orza8x9/TV_Cultura.png"
  }
];

const lista = document.querySelector("#listaCanais");
const busca = document.querySelector("#busca");
const categorias = document.querySelector("#categorias");
const vazio = document.querySelector("#vazio");

const modalPlayer = document.querySelector("#modalPlayer");
const video = document.querySelector("#video");
const playerNome = document.querySelector("#playerNome");
const playerStatus = document.querySelector("#playerStatus");
const tentarNovamente = document.querySelector("#tentarNovamente");

const modalCustom = document.querySelector("#modalCustom");
const customUrl = document.querySelector("#customUrl");

let categoriaAtual = "Todos";
let hls = null;
let streamAtual = "";

function iniciais(nome) {
  return nome.split(/\s+/).slice(0,2).map(p => p[0]).join("").toUpperCase();
}

function logoHTML(canal) {
  if (canal.imagem) {
    return `<img class="logo" src="${canal.imagem}" alt="" onerror="this.outerHTML='<div class=&quot;logo&quot; style=&quot;display:grid;place-items:center;font-weight:900&quot;>${iniciais(canal.nome)}</div>'">`;
  }
  return `<div class="logo" style="display:grid;place-items:center;font-weight:900">${iniciais(canal.nome)}</div>`;
}

function renderCategorias() {
  const cats = ["Todos", ...new Set(CANAIS.map(c => c.categoria))];
  categorias.innerHTML = cats.map(cat =>
    `<button class="chip ${cat === categoriaAtual ? "ativo" : ""}" data-cat="${cat}">${cat}</button>`
  ).join("");

  categorias.querySelectorAll("[data-cat]").forEach(btn => {
    btn.addEventListener("click", () => {
      categoriaAtual = btn.dataset.cat;
      renderCategorias();
      renderCanais();
    });
  });
}

function renderCanais() {
  const termo = busca.value.trim().toLowerCase();
  const filtrados = CANAIS.filter(c => {
    const okCat = categoriaAtual === "Todos" || c.categoria === categoriaAtual;
    const okBusca = !termo || c.nome.toLowerCase().includes(termo);
    return okCat && okBusca;
  });

  lista.innerHTML = filtrados.map(c => `
    <button class="canal" data-id="${c.id}">
      <div>${logoHTML(c)}</div>
      <div>
        <div class="canal-nome">${c.nome}</div>
        <div class="canal-cat">${c.categoria}</div>
      </div>
    </button>
  `).join("");

  vazio.hidden = filtrados.length > 0;

  lista.querySelectorAll("[data-id]").forEach(btn => {
    btn.addEventListener("click", () => {
      const canal = CANAIS.find(c => c.id === btn.dataset.id);
      abrirPlayer(canal.nome, canal.stream);
    });
  });
}

function limparPlayer() {
  if (hls) {
    hls.destroy();
    hls = null;
  }
  video.pause();
  video.removeAttribute("src");
  video.load();
}

function abrirPlayer(nome, url) {
  limparPlayer();
  streamAtual = url;
  playerNome.textContent = nome;
  playerStatus.textContent = "Carregando transmissão...";
  modalPlayer.hidden = false;

  if (!url) {
    playerStatus.textContent = "URL de transmissão não informada.";
    return;
  }

  if (video.canPlayType("application/vnd.apple.mpegurl")) {
    video.src = url;
    video.play().catch(() => {});
  } else if (window.Hls && Hls.isSupported()) {
    hls = new Hls({
      enableWorker: true,
      lowLatencyMode: true
    });
    hls.loadSource(url);
    hls.attachMedia(video);

    hls.on(Hls.Events.MANIFEST_PARSED, () => {
      playerStatus.textContent = "Transmissão carregada.";
      video.play().catch(() => {});
    });

    hls.on(Hls.Events.ERROR, (_, data) => {
      if (data.fatal) {
        playerStatus.textContent =
          "Não foi possível abrir este canal. O servidor pode bloquear HTTPS/CORS ou estar fora do ar.";
      }
    });
  } else {
    playerStatus.textContent = "Este navegador não oferece suporte a HLS.";
  }
}

busca.addEventListener("input", renderCanais);

document.querySelector("#fecharPlayer").addEventListener("click", () => {
  limparPlayer();
  modalPlayer.hidden = true;
});

tentarNovamente.addEventListener("click", () => {
  if (streamAtual) abrirPlayer(playerNome.textContent, streamAtual);
});

document.querySelector("#btnCustom").addEventListener("click", () => {
  modalCustom.hidden = false;
  customUrl.focus();
});

document.querySelector("#fecharCustom").addEventListener("click", () => {
  modalCustom.hidden = true;
});

document.querySelector("#abrirCustom").addEventListener("click", () => {
  const url = customUrl.value.trim();
  if (!url) return;
  modalCustom.hidden = true;
  abrirPlayer("URL personalizada", url);
});

renderCategorias();
renderCanais();
