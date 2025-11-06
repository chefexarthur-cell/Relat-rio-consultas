<!DOCTYPE html>
<html lang="pt-BR">
<head>
  <meta charset="utf-8" />
  <meta name="viewport" content="width=device-width,initial-scale=1" />
  <title>Relatório de Consulta — Hospital Israelita Albert Einstein</title>
  <style>
    body { font-family: Arial, sans-serif; margin:20px; background:#f4f4f4; }
    .container { max-width:700px; margin:auto; background:#fff; padding:20px; box-shadow:0 0 10px rgba(0,0,0,0.08); }
    h1,h2 { text-align:center; }
    .form-group { margin-bottom:12px; }
    label { display:block; margin-bottom:6px; font-weight:600; }
    input[type="text"], input[type="date"], input[type="time"], textarea { width:100%; padding:8px; box-sizing:border-box; }
    .actions { margin-top:12px; }
    .btn { padding:10px 16px; border:none; cursor:pointer; color:#fff; background:#007BFF; margin-right:8px; }
    .btn.secondary { background:#6c757d; }
    #conteudoRelatorio { border:1px solid #e6e6e6; padding:12px; background:#fafafa; margin-top:10px; }
    .hidden { display:none; }
    @media print {
      form, .actions { display:none !important; }
      body { background:#fff; }
      .container { box-shadow:none; margin:0; padding:0; }
    }
  </style>
</head>
<body>
  <div class="container">
    <h1>Relatório de Consulta</h1>
    <form id="relatorioForm" novalidate>
      <div class="form-group">
        <label for="nomeMedico">Nome do Médico:</label>
        <input type="text" id="nomeMedico" name="nomeMedico" required>
      </div>
      <div class="form-group">
        <label for="idMedico">ID do Médico:</label>
        <input type="text" id="idMedico" name="idMedico" required>
      </div>
      <div class="form-group">
        <label for="dataConsulta">Data da Consulta:</label>
        <input type="date" id="dataConsulta" name="dataConsulta" required>
      </div>
      <div class="form-group">
        <label for="horarioConsulta">Horário da Consulta:</label>
        <input type="time" id="horarioConsulta" name="horarioConsulta" required>
      </div>
      <div class="form-group">
        <label for="sintomas">Sintomas apresentados:</label>
        <textarea id="sintomas" name="sintomas" rows="3"></textarea>
      </div>
      <div class="form-group">
        <label for="ocorrido">Ocorrido (o que aconteceu):</label>
        <textarea id="ocorrido" name="ocorrido" rows="3" placeholder="Descreva o ocorrido, por exemplo: como começou, contexto, agravantes"></textarea>
      </div>
      <div class="form-group">
        <label for="medicamentos">Medicamentos prescritos:</label>
        <textarea id="medicamentos" name="medicamentos" rows="2"></textarea>
      </div>
      <div class="actions">
        <button type="submit" class="btn" id="btnGerar">Gerar Relatório</button>
        <button type="button" class="btn secondary" id="btnLimpar">Limpar</button>
      </div>
    </form>

    <div id="previewRelatorio" class="hidden" aria-live="polite" aria-atomic="true" tabindex="-1">
      <h2>Pré-visualização do Relatório</h2>
      <div id="conteudoRelatorio"></div>
      <div class="actions">
        <button class="btn" id="btnCopy">Copiar para Clipboard</button>
        <button class="btn" id="btnDownload">Baixar .txt</button>
        <button class="btn" id="btnPrint">Imprimir / Salvar como PDF</button>
      </div>
    </div>
  </div>

  <script>
    function escapeHtml(str) {
      if (!str) return '';
      return String(str)
        .replace(/&/g, '&amp;')
        .replace(/</g, '&lt;')
        .replace(/>/g, '&gt;')
        .replace(/"/g, '&quot;')
        .replace(/'/g, '&#039;');
    }

    function formatDatePTBR(dateString) {
      if (!dateString) return '';
      const d = new Date(dateString + 'T00:00:00');
      return d.toLocaleDateString('pt-BR');
    }

    function formatTime(timeString) {
      if (!timeString) return '';
      const [h, m] = timeString.split(':').map(Number);
      if (Number.isFinite(h) && Number.isFinite(m)) {
        return new Date(1970,0,1,h,m).toLocaleTimeString('pt-BR', {hour:'2-digit', minute:'2-digit'});
      }
      return timeString;
    }

    document.addEventListener('DOMContentLoaded', function () {
      const form = document.getElementById('relatorioForm');
      const preview = document.getElementById('previewRelatorio');
      const conteudoRelatorio = document.getElementById('conteudoRelatorio');
      const btnCopy = document.getElementById('btnCopy');
      const btnDownload = document.getElementById('btnDownload');
      const btnPrint = document.getElementById('btnPrint');
      const btnLimpar = document.getElementById('btnLimpar');

      function buildReportData() {
        const fd = new FormData(form);
        const nome = fd.get('nomeMedico') || '';
        const id = fd.get('idMedico') || '';
        const data = fd.get('dataConsulta') || '';
        const horario = fd.get('horarioConsulta') || '';
        const sintomas = fd.get('sintomas') || '';
        const ocorrido = fd.get('ocorrido') || '';
        const medicamentos = fd.get('medicamentos') || '';

        const dateFormatted = formatDatePTBR(data);
        const timeFormatted = formatTime(horario);

        // HTML Preview
        const html = `
          <p><strong>Nome do Médico:</strong> ${escapeHtml(nome)}</p>
          <p><strong>ID Médico:</strong> ${escapeHtml(id)}</p>
          <p><strong>Data da Consulta:</strong> ${escapeHtml(dateFormatted)}</p>
          <p><strong>Horário:</strong> ${escapeHtml(timeFormatted)}</p>
          <p><strong>Sintomas:</strong><br>${escapeHtml(sintomas).replace(/\n/g, '<br>')}</p>
          <p><strong>Ocorrido:</strong><br>${escapeHtml(ocorrido).replace(/\n/g, '<br>')}</p>
          <p><strong>Medicamentos:</strong><br>${escapeHtml(medicamentos).replace(/\n/g, '<br>')}</p>
        `;

        // Modelo Profissional para clipboard/texto
        const profissional = [
          "🇮🇱🇮🇱 HOSPITAL ISRAELITA ALBERT EINSTEIN 🇮🇱🇮🇱",
          "Relatório de Consulta Médica",
          "───────────────────────────────",
          `👨‍⚕️ Médico Responsável: ${nome}`,
          `🆔 ID do Médico: ${id}`,
          `📅 Data da Consulta: ${dateFormatted}`,
          `⏰ Horário: ${timeFormatted}`,
          "───────────────────────────────",
          "🩺 Sintomas apresentados:",
          sintomas ? sintomas : "(não informado)",
          "",
          "📋 Ocorrido (descrição):",
          ocorrido ? ocorrido : "(não informado)",
          "",
          "💊 Medicamentos prescritos:",
          medicamentos ? medicamentos : "(não informado)",
          "───────────────────────────────",
          "Este relatório é confidencial e destinado exclusivamente ao uso médico.",
          "🇮🇱 Hospital Israelita Albert Einstein 🇮🇱"
        ].join('\n');

        return { html, profissional };
      }

      form.addEventListener('submit', function (e) {
        e.preventDefault();
        if (!form.checkValidity()) {
          form.reportValidity();
          return;
        }
        const { html, profissional } = buildReportData();
        conteudoRelatorio.innerHTML = html;
        conteudoRelatorio.dataset.profissional = profissional;
        preview.classList.remove('hidden');
        preview.focus();
      });

      btnCopy.addEventListener('click', function () {
        const text = conteudoRelatorio.dataset.profissional || conteudoRelatorio.innerText || '';
        if (navigator.clipboard && navigator.clipboard.writeText) {
          navigator.clipboard.writeText(text).then(function () {
            alert('Relatório profissional copiado para o clipboard.');
          }).catch(function (err) {
            fallbackCopyTextToClipboard(text);
          });
        } else {
          fallbackCopyTextToClipboard(text);
        }
      });

      function fallbackCopyTextToClipboard(text) {
        const el = document.createElement('textarea');
        el.value = text;
        el.style.position = 'fixed';
        el.style.left = '-9999px';
        document.body.appendChild(el);
        el.select();
        try {
          document.execCommand('copy');
          alert('Relatório profissional copiado para o clipboard (fallback).');
        } catch (err) {
          alert('Não foi possível copiar para o clipboard.');
        }
        document.body.removeChild(el);
      }

      btnDownload.addEventListener('click', function () {
        const text = conteudoRelatorio.dataset.profissional || conteudoRelatorio.innerText || '';
        const blob = new Blob([text], { type: 'text/plain;charset=utf-8' });
        const url = URL.createObjectURL(blob);
        const a = document.createElement('a');
        const now = new Date();
        const timestamp = now.toISOString().slice(0,19).replace(/[:T]/g,'-');
        a.href = url;
        a.download = `relatorio-consulta-einstein-${timestamp}.txt`;
        document.body.appendChild(a);
        a.click();
        document.body.removeChild(a);
        URL.revokeObjectURL(url);
      });

      btnPrint.addEventListener('click', function () {
        window.print();
      });

      btnLimpar.addEventListener('click', function () {
        form.reset();
        conteudoRelatorio.innerHTML = '';
        delete conteudoRelatorio.dataset.profissional;
        preview.classList.add('hidden');
      });
    });
  </script>
</body>
</html>
