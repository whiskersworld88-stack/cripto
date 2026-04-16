import pandas as pd
import requests
from io import StringIO
from flask import Flask, jsonify, render_template

app = Flask(__name__, template_folder='templates')

# Link CSV Anda sudah terpasang di sini
SHEET_CSV_URL = "https://docs.google.com/spreadsheets/d/e/2PACX-1vQye0iX3Sh2qkrb8WIfk_IEyE6DKITI_r8y6yOJN0lmT6ggyA1-IgFzmL7dJ2aedNjgm-n2wmm34Egc/pub?output=csv"

def fetch_data():
    try:
        print("Fetching data from Google Sheets...")
        response = requests.get(SHEET_CSV_URL)
        response.raise_for_status()
        response.encoding = 'utf-8'
        
        # Membaca data dengan encoding utf-8
        df = pd.read_csv(StringIO(response.text))
        
        # Bersihkan nama kolom dari spasi atau karakter aneh
        df.columns = df.columns.str.strip()
        
        print(f"Columns found: {df.columns.tolist()}")
        print(f"Data shape: {df.shape}")
        
        final_data = []
        for _, row in df.iterrows():
            try:
                # Mengambil data sesuai header di Sheets Anda
                ticker = str(row.get('Ticker', '-'))
                price = float(row.get('Price', 0))
                
                # Menangani format persentase di kolom Change%
                chg_raw = str(row.get('Change%', '0')).replace('%', '')
                chg_pct = float(chg_raw) if chg_raw else 0
                
                pe = float(row.get('PE', 0)) if pd.notna(row.get('PE', 0)) else 0
                mcap = float(row.get('MarketCap', 0)) if pd.notna(row.get('MarketCap', 0)) else 0
                vol = float(row.get('Volume', 0)) if pd.notna(row.get('Volume', 0)) else 0
                avg_vol = float(row.get('Avgvolume', 0)) if pd.notna(row.get('Avgvolume', 0)) else 0

                # --- LOGIKA SINYAL SOVEREIGN ---
                score = 0
                
                # 1. Analisis Volume (Deteksi Akumulasi)
                if avg_vol > 0:
                    ratio = vol / avg_vol
                    if ratio > 2.0: 
                        score += 4  # Volume sangat tinggi
                    elif ratio > 1.2: 
                        score += 2  # Volume di atas rata-rata
                
                # 2. Analisis Valuasi (P/E Ratio)
                if 0 < pe < 10: 
                    score += 3     # Undervalued
                elif 10 <= pe < 18: 
                    score += 1     # Fair value
                
                # 3. Analisis Momentum (Price Change)
                if chg_pct > 3: 
                    score += 2     # Bullish kuat
                elif chg_pct > 0: 
                    score += 1     # Positive momentum

                # Penentuan Status Berdasarkan Total Skor
                if score >= 7: 
                    status = "🔥 STRONG BUY"
                elif score >= 4: 
                    status = "✅ BUY"
                elif score >= 2: 
                    status = "⏳ WATCH"
                else: 
                    status = "💀 AVOID"

                final_data.append({
                    'ticker': ticker,
                    'price': f"{price:,.0f}",
                    'change_pct': f"{chg_pct:+.2f}%",
                    'mcap': f"{mcap/1e12:.2f}T" if mcap > 0 else "0T",
                    'pe': f"{pe:.1f}" if pe > 0 else "N/A",
                    'vol_ratio': f"{vol/avg_vol:.1f}x" if avg_vol > 0 else "N/A",
                    'status': status,
                    'score': score
                })
            except Exception as e:
                print(f"Error pada baris {row.get('Ticker', 'unknown')}: {e}")
                continue
                
        print(f"Successfully processed {len(final_data)} rows")
        return final_data
    except Exception as e:
        print(f"Error Fetching Data: {e}")
        import traceback
        traceback.print_exc()
        return None

@app.route('/')
def index():
    return render_template('index.html')

@app.route('/api/signals')
def get_signals():
    data = fetch_data()
    if data:
        return jsonify(data)
    return jsonify({"error": "Failed to fetch data"}), 500

if __name__ == '__main__':
    app.run(debug=True, port=5000)
