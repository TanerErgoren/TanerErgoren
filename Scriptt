from flask import Flask, request, jsonify, render_template
from flask_login import LoginManager, UserMixin, login_user, login_required, logout_user, current_user
import time
import random
import requests
import matplotlib.pyplot as plt
import io
import base64

app = Flask(__name__)
app.secret_key = "supersecretkey"

login_manager = LoginManager()
login_manager.init_app(app)
login_manager.login_view = "login"

# Kullanıcı veritabanı (test amaçlı)
users = {
    "admin": {"password": "1234", "role": "admin", "balance": 1000},
    "user1": {"password": "abcd", "role": "user", "balance": 500}
}

class User(UserMixin):
    def __init__(self, username, role):
        self.id = username
        self.role = role

@login_manager.user_loader
def load_user(username):
    if username in users:
        return User(username, users[username]["role"])
    return None

@app.route("/")
def home():
    return "Merhaba! Flask Web Siteniz Çalışıyor 🚀"

@app.route("/dashboard")
@login_required
def dashboard():
    balances = {user: users[user]["balance"] for user in users}
    fig, ax = plt.subplots()
    ax.bar(balances.keys(), balances.values(), color=['blue', 'green'])
    ax.set_ylabel("Bakiye (EUR)")
    ax.set_title("Kullanıcı Bakiye Dağılımı")

    img = io.BytesIO()
    plt.savefig(img, format='png')
    img.seek(0)
    graph_url = base64.b64encode(img.getvalue()).decode()

    return f'<img src="data:image/png;base64,{graph_url}"/>'

if __name__ == "__main__":
    app.run(host="0.0.0.0", port=3000)
