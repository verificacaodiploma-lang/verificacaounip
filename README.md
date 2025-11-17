<!doctype html>
<html lang="pt-BR">

<head>
  <meta charset="utf-8" />
  <meta name="viewport" content="width=device-width,initial-scale=1" />
  <title>UNIP — Sistema de Verificação de Diplomas</title>
  <style>
    :root {
      --bg: #f5f7ff;
      --card: #ffffff;
      --accent: #003399; /* Azul bic */
      --muted: #6b7280;
      --gold: #FFD700; /* Amarelo */
    }

    body {
      font-family: Inter, system-ui, -apple-system, 'Segoe UI', Roboto, 'Helvetica Neue', Arial;
      background: var(--bg);
      margin: 0;
      color: #0f172a;
    }

    header {
      background: linear-gradient(90deg, #00339920, #FFD70020);
      padding: 24px 28px;
      border-bottom: 3px solid var(--gold);
      display: flex;
      align-items: center;
      justify-content: space-between;
    }

    .brand {
      font-weight: 700;
      font-size: 18px;
      color: var(--accent);
    }

    .container {
      max-width: 980px;
      margin: 36px auto;
      padding: 20px;
    }

    .search {
      background: var(--card);
      border-radius: 12px;
      padding: 20px;
      box-shadow: 0 6px 18px rgba(15, 23, 42, 0.08);
      display: flex;
      gap: 16px;
      align-items: center;
    }

    .search input {
      flex: 1;
      padding: 14px 16px;
      border: 1px solid #d6d9e5;
      border-radius: 8px;
      font-size: 15px;
    }

    .search button {
      background: var(--accent);
      color: white;
      border: none;
      padding: 12px 16px;
      border-radius: 8px;
      font-weight: 600;
      cursor: pointer;
    }

    .search button:hover {
      background: #00246b;
    }

    .content {
      display: flex;
      gap: 20px;
      margin-top: 20px;
    }

    .left {
      flex: 1;
    }

    .right {
      width: 460px;
    }

    .card {
      background: var(--card);
      border-radius: 12px;
      padding: 22px;
      box-shadow: 0 6px 18px rgba(15, 23, 42, 0.08);
    }

    .muted {
      color: var(--muted);
      font-size: 14px;
    }

    .img-wrap {
      background: #fffbe8;
      padding: 18px;
      border-radius: 10px;
      border: 1px solid #ffe38a;
    }

    .meta {
      margin-top: 12px;
      font-size: 15px;
    }

    .label {
      display: inline-block;
      padding: 6px 10px;
      border-radius: 8px;
      background: var(--gold);
      color: var(--accent);
      font-weight: 700;
    }

    footer {
      max-width: 980px;
      margin: 28px auto;
      color: var(--muted);
      font-size: 13px;
      text-align: center;
    }

    @media (max-width:820px) {
      .content {
        flex-direction: column;
      }

      .right {
        width: 100%;
      }
    }

    #previewCanvas {
      max-width: 100%;
      border-radius: 8px;
      box-shadow: 0 8px 30px rgba(12, 20, 40, 0.06);
      background: white;
    }
  </style>
</head>

<body>
  <header>
    <div class="brand" id="brand">UNIP — Sistema de Verificação de Diplomas</div>
    <div class="muted" id="subtitle">Universidade Paulista</div>
  </header>

  <div class="container">
    <div class="search" role="search" aria-label="Pesquisar diploma">
      <input id="q" placeholder="Consultar por número do diploma (ex: 974233-91)" aria-label="Número do diploma" />
      <button id="btn">Consultar</button>
    </div>

    <div class="content">
      <div class="left">
        <div class="card" id="result">
          <div class="muted">Resultado da verificação</div>
          <h2 id="name">—</h2>
          <div class="meta" id="degree">—</div>
          <div class="meta" id="inst">—</div>
          <div class="meta" id="date">—</div>
          <div style="margin-top:14px;">
            <span class="label" id="status">—</span>
          </div>
          <hr style="margin:18px 0;border:none;border-top:1px solid #eef2f6" />
          <div class="muted">Número do diploma</div>
          <div id="dnum" style="font-weight:700;margin-top:6px">—</div>
        </div>
      </div>

      <div class="right">
        <div class="card">
          <div class="muted">Visualização</div>
          <div class="img-wrap" style="margin-top:12px;display:flex;justify-content:center">
            <canvas id="previewCanvas" width="420" height="260" role="img"
              aria-label="Pré-visualização do diploma"></canvas>
          </div>
        </div>
      </div>
    </div>

  </div>

  <footer id="footerText">
    UNIP — © 2025
  </footer>

  <script>
    const records = {
      '974233-91': {
        diploma: '974233-91',
        name: 'Larissa Cristina de Andrade Silva',
        degree: 'Bacharel em Administração',
        inst: 'Universidade Paulista (UNIP)',
        date: 'Conclusão em 22/11/2024',
        status: 'Ativo'
      },
      '46886-75': {
        diploma: '46886-75',
        name: 'Alex Aparecido Fernandes Rosseti',
        degree: 'Bacharel em Psicologia',
        inst: 'Universidade Paulista (UNIP)',
        date: 'Conclusão em 10/08/2024',
        status: 'Ativo'
      },
      '46556-54': {
        diploma: '46556-54',
        name: 'Josilene Santos Ribeiro',
        degree: 'Técnico em Enfermagem',
        inst: 'Universidade Paulista (UNIP)',
        date: 'Conclusão em 07/08/2024',
        status: 'Ativo'
      },

      '47755-66': {
        diploma: '47755-66',
        name: 'Josilene Santos Ribeiro',
        degree: 'Licenciatura em Pedagogia',
        inst: 'Universidade Paulista (UNIP)',
        date: 'Conclusão em 20/08/2024',
        status: 'Ativo'
      },

      /* 🔥 NOVO REGISTRO — Tatiane */
      '57775-86': {
        diploma: '57775-86',
        name: 'Tatiane Carla Valdivino Gomes Silva',
        degree: 'Licenciatura em Pedagogia',
        inst: 'Universidade Paulista (UNIP)',
        date: 'Conclusão em 26/02/2021',
        status: 'Ativo'
      }
    };

    document.getElementById('btn').addEventListener('click', function () {
      const q = document.getElementById('q').value.trim();
      const result = records[q];
      const brand = document.getElementById('brand');
      const footerText = document.getElementById('footerText');

      if (result) {
        document.getElementById('name').innerText = result.name;
        document.getElementById('degree').innerText = result.degree;
        document.getElementById('inst').innerText = result.inst;
        document.getElementById('date').innerText = result.date;
        document.getElementById('status').innerText = result.status;
        document.getElementById('dnum').innerText = result.diploma;

        brand.innerText = `${result.inst} — Sistema de Verificação de Diplomas`;
        footerText.innerText = `${result.inst} — © 2025`;
      } else {
        alert('Diploma não encontrado. Verifique o número digitado.');
        document.getElementById('name').innerText = '—';
        document.getElementById('degree').innerText = '—';
        document.getElementById('inst').innerText = '—';
        document.getElementById('date').innerText = '—';
        document.getElementById('status').innerText = '—';
        document.getElementById('dnum').innerText = '—';

        brand.innerText = `UNIP — Sistema de Verificação de Diplomas`;
        footerText.innerText = `UNIP — © 2025`;
      }
    });
  </script>
</body>

</html>
