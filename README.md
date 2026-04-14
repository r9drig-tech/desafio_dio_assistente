# desafio_dio_assistente

# Desafio DIO: Assistente Virtual Neymar Jr - Full Stats por Clube
import pandas as pd
import os

class NeyBot:
    def __init__(self, data_path):
        self.nome = "NeyBot Stats"
        self.df = pd.DataFrame()
        
        if os.path.exists(data_path):
            try:
                # Carrega o CSV pulando a linha de título e detectando separador (vírgula ou ponto e vírgula)
                self.df = pd.read_csv(data_path, skiprows=1, sep=None, engine='python')
                self.df.columns = self.df.columns.str.strip()
                print(f"[{self.nome}]: Base de dados carregada! Sistema pronto para análise detalhada.")
            except Exception as e:
                print(f"[{self.nome}]: Erro ao carregar dados: {e}")
        else:
            print(f"[{self.nome}]: Ficheiro não encontrado.")

    def responder_consulta(self, pergunta):
        if self.df.empty: return "Base de dados indisponível."
        pergunta = pergunta.lower()
        
        try:
            # 1. ANATOMIA POR CLUBE (Direita, Esquerda, Cabeça)
            if "perna" in pergunta or "direita" in pergunta or "esquerda" in pergunta or "cabeça" in pergunta:
                res = "🧬 Distribuição Anatômica por Clube:\n"
                for _, row in self.df.iterrows():
                    res += (f"- {row['Clube']}: Direita ({row['Perna_Direita']}) | "
                            f"Esquerda ({row['Perna_Esquerda']}) | Cabeça ({row['Cabeca']})\n")
                return res

            # 2. ESPECIALIDADES POR CLUBE (Falta e Pênalti)
            elif "falta" in pergunta or "pênalti" in pergunta or "penalti" in pergunta:
                res = "🎯 Gols de Bola Parada por Clube:\n"
                for _, row in self.df.iterrows():
                    res += f"- {row['Clube']}: Falta ({row['Gols_Falta']}) | Pênalti ({row['Gols_Penalti']})\n"
                return res

            # 3. ASSISTÊNCIAS POR CLUBE
            elif "assistência" in pergunta or "assistencia" in pergunta or "passe" in pergunta:
                res = "👟 Assistências por Clube:\n"
                for _, row in self.df.iterrows():
                    res += f"- {row['Clube']}: {row['Assistencias']} assistências\n"
                return res

            # 4. GOLS TOTAIS POR CLUBE
            elif "clube" in pergunta or "time" in pergunta:
                res = "📊 Gols Totais por Clube:\n"
                for _, row in self.df.iterrows():
                    res += f"- {row['Clube']}: {row['Gols_Totais']} gols\n"
                return res

            # 5. RESUMO GERAL
            elif "resumo" in pergunta or "tudo" in pergunta:
                return (f"📝 RESUMO GERAL (TOTAL):\n"
                        f"- Gols Totais: {self.df['Gols_Totais'].sum()}\n"
                        f"- Assistências: {self.df['Assistencias'].sum()}\n"
                        f"- Perna Direita: {self.df['Perna_Direita'].sum()}\n"
                        f"- Perna Esquerda: {self.df['Perna_Esquerda'].sum()}\n"
                        f"- Cabeça: {self.df['Cabeca'].sum()}\n"
                        f"- Faltas: {self.df['Gols_Falta'].sum()}\n"
                        f"- Pênaltis: {self.df['Gols_Penalti'].sum()}")

            else:
                return "Pergunte por: gols, assistências, faltas, pênaltis ou anatomia (pernas/cabeça) por clube."
        
        except KeyError as e:
            return f"Erro: A coluna {e} não foi encontrada no Excel. Verifique os nomes das colunas."

# CAMINHO ABSOLUTO
caminho_ficheiro = r'C:\Pyhton-script\neymar_dashboard_final_2026.csv'
assistente = NeyBot(caminho_ficheiro)

print("-" * 50)
if not assistente.df.empty:
    # Testes automáticos para você ver o resultado no terminal
    print(f"Assistente: {assistente.responder_consulta('Quais os gols de perna e cabeça por clube?')}")
    print("-" * 50)
    print(f"Assistente: {assistente.responder_consulta('Me mostre os gols de falta e penalti por time?')}")
    print("-" * 50)
    print(f"Assistente: {assistente.responder_consulta('Dê um resumo de tudo?')}")
