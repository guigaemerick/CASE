# CASE

import streamlit as st
import pandas as pd

# ---------------------------------------------------------
# Configuração da Página
# ---------------------------------------------------------
st.set_page_config(
    page_title="Painel de Oportunidades | LIA UFSCar",
    page_icon="🤖",
    layout="wide",
    initial_sidebar_state="expanded"
)

# ---------------------------------------------------------
# Cabeçalho
# ---------------------------------------------------------
st.title("Painel de Oportunidades — LIA UFSCar")
st.caption(
    "💡 **Nota de Gestão de Capacidade:** Nossa equipe é composta por apenas **6 membros**. "
    "Todas as priorizações e decisões estratégicas deste painel visam equilibrar a entrega de "
    "valor institucional com a proteção da saúde e bem-estar do time."
)

st.divider()

# ---------------------------------------------------------
# Métricas Rápidas (KPIs)
# ---------------------------------------------------------
col1, col2, col3, col4 = st.columns(4)
with col1:
    st.metric(label="Total de Oportunidades", value="6")
with col2:
    st.metric(label="Execução Direta", value="2", delta="Foco imediato", delta_color="normal")
with col3:
    st.metric(label="Relacionamento / 2027", value="3", delta="Baixo esforço", delta_color="normal")
with col4:
    st.metric(label="Declinadas", value="1", delta="Prevenção de sobrecarga", delta_color="inverse")

st.write("")

# ---------------------------------------------------------
# Base de Dados (Pandas)
# ---------------------------------------------------------
dados = {
    "Organização": [
        "Meridian",
        "Senda",
        "Violeta",
        "Brava",
        "Prisma",
        "Aerion"
    ],
    "Prazo de Retorno": [
        "2026-10-01",
        "2026-10-02",
        "2026-10-05",
        "2026-10-07",
        "2026-10-15",
        "2026-10-16"
    ],
    "Decisão / Prioridade": [
        "Executar (Oficina)",
        "Executar (Piloto)",
        "Explorar (Conversa)",
        "Declinar (Sobrecarga)",
        "Explorar (Conversa)",
        "Cultivar para 2027"
    ],
    "Dedicação Estimada": [
        "Baixa (1 manhã)",
        "Alta (2 membros/4 sem)",
        "Baixa (30 min)",
        "Zero",
        "Baixa (20 min)",
        "Baixa (Visita)"
    ],
    "Responsável (Área)": [
        "Relações Inst.",
        "Operações",
        "Relações Inst.",
        "Relações Inst.",
        "Relações Inst.",
        "Relações Inst."
    ],
    "Próxima Ação": [
        "Confirmar presença",
        "Confirmar interesse",
        "Fazer call",
        "Agradecer e declinar",
        "Enviar docs LIA",
        "Confirmar interesse"
    ]
}

df = pd.DataFrame(dados)

# 1. Conversão para datetime
df["Prazo de Retorno"] = pd.to_datetime(df["Prazo de Retorno"])

# 2. Ordenação por urgência (prazo mais próximo ao mais distante)
df = df.sort_values(by="Prazo de Retorno", ascending=True).reset_index(drop=True)

# 3. Formatação da data para padrão brasileiro (DD/MM/YYYY)
df_display = df.copy()
df_display["Prazo de Retorno"] = df_display["Prazo de Retorno"].dt.strftime("%d/%m/%Y")

# ---------------------------------------------------------
# Exibição da Tabela
# ---------------------------------------------------------
st.subheader("📋 Fila de Oportunidades Priorizadas")

st.dataframe(
    df_display,
    use_container_width=True,
    hide_index=True,
    column_config={
        "Organização": st.column_config.TextColumn("Organização", width="medium"),
        "Prazo de Retorno": st.column_config.TextColumn("Prazo de Retorno", width="small"),
        "Decisão / Prioridade": st.column_config.TextColumn("Decisão / Prioridade", width="medium"),
        "Dedicação Estimada": st.column_config.TextColumn("Dedicação Estimada", width="medium"),
        "Responsável (Área)": st.column_config.TextColumn("Responsável (Área)", width="small"),
        "Próxima Ação": st.column_config.TextColumn("Próxima Ação", width="medium"),
    }
)

st.write("")

# ---------------------------------------------------------
# Justificativas Estratégicas (Seções Expansíveis)
# ---------------------------------------------------------
st.subheader("📌 Justificativas & Pendências de Transição")

with st.expander("🚀 Execução — Senda e Meridian", expanded=False):
    st.markdown("""
    - **Racional Estratégico:** Estas iniciativas foram priorizadas por gerarem **portfólio rápido** e **alto valor educacional** prático para os membros, mantendo a curva de esforço plenamente viável dentro do semestre letivo.
    - **Informações Pendentes & Próximos Passos:**
      - **Senda (Piloto):** Aguarda aprovação interna e definição do protocolo de mascaramento/anonimização de dados confidenciais antes do início dos testes.
      - **Meridian (Oficina):** Necessário alinhar a data final do workshop e confirmar a disponibilidade da equipe para a data selecionada.
    """)

with st.expander("🤝 Relacionamento — Violeta, Prisma e Aerion", expanded=False):
    st.markdown("""
    - **Racional Estratégico:** No momento, a Liga não possui capacidade operacional para absorver projetos extensos sem comprometer o semestre da equipe.
    - **Objetivo Atual:** O foco está estritamente em realizar **reuniões exploratórias de baixo custo de tempo** (20 a 30 minutos ou visita institucional) para estabelecer contato institucional, manter portas abertas e criar tração para **parcerias em 2027**.
    """)

with st.expander("🛑 Renúncia — Brava", expanded=False):
    st.markdown("""
    - **Racional Estratégico:** A oportunidade foi formalmente declinada devido ao **alto risco de sobrecarga**.
    - **Impacto no Time:** O escopo demandaria mais da metade da equipe (pelo menos 4 de 6 membros) ao longo de 6 semanas de dedicação contínua, sem a contrapartida de gerar um case de mercado expressivo ou aprendizado técnico proporcional para a Liga. A recusa protege o fôlego operacional do grupo.
    """)
