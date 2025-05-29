import streamlit as st

def estimate_pumping_volume(C, P, eta, H, A):
    Q = (P * eta) / (0.222 * H * 1.2)
    V = (Q * 60 * C) / (2 * (P * A))
    return Q, V

st.set_page_config(page_title="農業抽水量估算工具", page_icon="💧")

# ✅ 加入新的標題與研究說明文字
st.title("🌱💧 作物月需水量推估工具（以電費為基礎）")

st.markdown("""
本研究開發本工具，旨在透過收集農民每月用電資料，  
結合抽水設備效率、水頭深度與耕作面積，  
**推估各類農作物的每月實際需水量**。

🔍 **研究目的**:
以電費作為核心參數，建立一個低成本、高可行性的推估模型，  
解決農業現場缺乏即時用水監測的問題，支援政府地下水資源管理。

🧮 **模型依據**:
本工具採用下列兩階段公式：

1. 計算抽水機每分鐘流量：  
$Q = \\frac{P \\cdot \\eta}{0.222 \\cdot H \\cdot 1.2}$

2. 推估每月每分地的抽水總量（代表作物需水量）：  
$V = \\frac{Q \\cdot 60 \\cdot C}{2 \\cdot (P \\cdot A)}$

📌 **應用情境**:
- 無需安裝流量計，即可評估作物用水行為  
- 適用於水資源管控區、地下水超抽區與智慧農業導入情境  
- 可搭配農民用電帳單與地籍圖進行分區分析

---
👉 請輸入各參數，即可獲得「推估月需水量」
""")

# ✅ 使用者輸入
C = st.number_input("每月用電量 C (kWh)", min_value=0.0, value=500.0)
P = st.number_input("抽水馬達馬力 P (HP)", min_value=0.1, value=10.0)
eta = st.slider("抽水效率 η (0 ~ 1)", 0.0, 1.0, 0.8)
H = st.number_input("水頭深度 H (公尺)", min_value=0.1, value=15.0)
A = st.number_input("耕作面積 A (公頃)", min_value=0.01, value=5.0)

# ✅ 執行計算
if P > 0 and H > 0 and A > 0 and eta > 0:
    Q, V = estimate_pumping_volume(C, P, eta, H, A)
    st.success(f"💧 每分鐘抽水流量 Q = {Q:.2f} 立方米/分鐘")
    st.success(f"🌱 作物每月需水量 V = {V:.2f} 立方米/分地")
else:
    st.warning("請輸入有效的正數參數")

st.caption("公式: V = (Q*60*C) / (2*(P*A))")
# app.py
