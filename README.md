# Projeto-Agrinho-2026

import requests

def buscar_dados_ana():
    print("🤖 Conectando à API pública da ANA...")
    # URL da API de Telemetria da ANA (estações de monitoramento de rios)
    url = "https://snirh.gov.br"
    
    try:
        response = requests.get(url, timeout=10)
        if response.status_code == 200:
            # Retorna as primeiras 3 estações encontradas para demonstração
            return response.json().get('content', [])[:3]
        else:
            print(f"❌ Erro ao acessar a API: Código {response.status_code}")
            return None
    except Exception as e:
        print(f"❌ Falha na conexão: {e}")
        return None

def analisar_viabilidade_agro(estacao):
    # Simulando a análise com base nos dados que você coletará dos sensores
    # Em um cenário real, você usaria os campos de pH e poluentes vindos da API
    status_estacao = estacao.get('status', 1)
    
    if status_estacao == 1:
        return "🟢 SEGURA (pH estável, sem detecção de agrotóxicos. Ideal para o Agro)"
    elif status_estacao == 2:
        return "🟡 ALERTA (Presença de resíduos de fertilizantes. Use com atenção)"
    else:
        return "🔴 INADEQUADA (Alta salinidade ou contaminação. Risco para plantio/gado)"

def calcular_impacto_agronegocio(litros_rio):
    # Dado do texto base: 1 kg de carne bovina gasta 15.400 litros
    litros_por_kg_carne = 15400
    if litros_rio > 0:
        kg_produzidos = litros_rio / litros_por_kg_carne
        return round(kg_produzidos, 2)
    return 0

def rodar_painel_monitoramento():
    print("=" * 60)
    print("       🌾 PLATAFORMA DE MONITORAMENTO ECO-AGRO 🌾       ")
    print("=" * 60)
    
    dados_rio = buscar_dados_ana()
    
    if not dados_rio:
        print("Não foi possível carregar os dados públicos neste momento.")
        return

    for estacao in dados_rio:
        nome_rio = estacao.get('nome', 'Rio Sem Nome')
        bacia = estacao.get('baciaNome', 'Não informada')
        
        # Simulando uma vazão fictícia para cálculo (já que depende da leitura do dia)
        vazao_simulada_litros = 500000 
        kg_carne_equivalente = calcular_impacto_agronegocio(vazao_simulada_litros)
        
        print(f"\n📍 Rio/Estação: {nome_rio} (Bacia: {bacia})")
        print(f"📊 Qualidade da Água: {analisar_viabilidade_agro(estacao)}")
        print(f"💧 Consumo Relativo: A vazão atual deste ponto equivale ao gasto")
        print(f"   necessário para produzir {kg_carne_equivalente} kg de carne bovina.")
        print("-" * 60)

if __name__ == "__main__":
    rodar_painel_monitoramento()
           
