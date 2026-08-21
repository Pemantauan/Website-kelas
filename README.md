<!DOCTYPE html>
<html lang="id">
<head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>XII TJKT 3 - Kelas Digital</title>
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.0.0-beta3/css/all.min.css" />
    <style>
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
            font-family: 'Segoe UI', Roboto, system-ui, sans-serif;
        }
        html { scroll-behavior: smooth; }
        body {
            background: #f4f7fc;
            color: #1e293b;
            line-height: 1.6;
            padding: 1.5rem;
            transition: all 0.3s ease;
        }

        #loadingOverlay {
            position: fixed;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            background: #0b3b5c;
            display: flex;
            flex-direction: column;
            justify-content: center;
            align-items: center;
            z-index: 9999;
            transition: opacity 0.8s ease, visibility 0.8s ease;
        }
        #loadingOverlay.hidden {
            opacity: 0;
            visibility: hidden;
            pointer-events: none;
        }
        .loading-container { text-align: center; max-width: 600px; padding: 2rem; }
        .loading-title { color: white; font-size: 3.8rem; font-weight: 900; letter-spacing: 4px; text-shadow: 0 4px 30px rgba(0,0,0,0.2); margin-bottom: 0.2rem; }
        .loading-title .highlight { background: rgba(255,255,255,0.08); padding: 0.1rem 1.8rem; border-radius: 16px; display: inline-block; border: 1px solid rgba(255,255,255,0.06); }
        .loading-sub { color: rgba(255,255,255,0.5); font-size: 1rem; font-weight: 300; letter-spacing: 2px; margin-bottom: 0.5rem; }
        .loading-divider { width: 80px; height: 2px; background: rgba(255,255,255,0.15); margin: 1rem auto 1.5rem; border-radius: 2px; }
        .loading-desc { color: rgba(255,255,255,0.6); font-size: 0.95rem; font-weight: 300; letter-spacing: 1px; margin-bottom: 2rem; }
        .loader-wrapper { position: relative; width: 70px; height: 70px; margin: 0 auto 1.5rem; }
        .loader-wrapper::before { content: ''; position: absolute; top: -6px; left: -6px; right: -6px; bottom: -6px; border-radius: 50%; border: 2px solid rgba(255,255,255,0.05); animation: pulse-ring 1.5s ease-in-out infinite; }
        @keyframes pulse-ring { 0% { transform: scale(1); opacity: 0.6; } 50% { transform: scale(1.15); opacity: 0.1; } 100% { transform: scale(1); opacity: 0.6; } }
        .loader { width: 70px; height: 70px; border: 3.5px solid rgba(255,255,255,0.1); border-top: 3.5px solid #ffffff; border-radius: 50%; animation: spin 1s linear infinite; box-shadow: 0 0 50px rgba(255,255,255,0.05); }
        @keyframes spin { 0% { transform: rotate(0deg); } 100% { transform: rotate(360deg); } }
        .loader-text { color: rgba(255,255,255,0.5); margin-top: 0.5rem; font-size: 0.9rem; font-weight: 300; letter-spacing: 3px; }
        .loader-text span { display: inline-block; animation: dot 1.4s infinite; }
        .loader-text span:nth-child(2) { animation-delay: 0.2s; }
        .loader-text span:nth-child(3) { animation-delay: 0.4s; }
        @keyframes dot { 0%, 80%, 100% { opacity: 0; } 40% { opacity: 1; } }
        .loading-footer { margin-top: 2.5rem; color: rgba(255,255,255,0.3); font-size: 0.85rem; letter-spacing: 1px; font-weight: 300; }
        .loading-footer strong { color: rgba(255,255,255,0.5); font-weight: 400; }

        /* ===== LOGIN ===== */
        .login-page {
            display: flex;
            justify-content: center;
            align-items: center;
            min-height: 100vh;
            background: linear-gradient(135deg, #e1edf7 0%, #b8d0e5 100%);
            padding: 1.5rem;
        }
        .login-box {
            background: white;
            border-radius: 2.5rem;
            padding: 3rem 2.5rem;
            max-width: 420px;
            width: 100%;
            box-shadow: 0 20px 60px -12px rgba(11, 59, 92, 0.3);
            text-align: center;
            animation: fadeUp 0.6s ease;
        }
        @keyframes fadeUp { from { opacity: 0; transform: translateY(30px); } to { opacity: 1; transform: translateY(0); } }
        .login-box .logo-icon { background: #0b3b5c; color: white; width: 70px; height: 70px; border-radius: 20px; display: flex; align-items: center; justify-content: center; font-size: 2.5rem; font-weight: 700; margin: 0 auto 1rem; box-shadow: 0 8px 20px rgba(11, 59, 92, 0.2); }
        .login-box h1 { font-size: 1.8rem; color: #0a2a42; font-weight: 700; }
        .login-box .subtitle { color: #4b6a89; font-size: 0.95rem; margin-bottom: 0.5rem; }
        .login-box .subtitle .by { font-size: 0.8rem; color: #7a94b0; margin-top: 0.2rem; }
        .login-box .subtitle .by strong { color: #0b3b5c; }

        .login-tabs {
            display: flex;
            gap: 0;
            margin-bottom: 1.5rem;
            border-radius: 60px;
            background: #e9eef3;
            padding: 4px;
        }
        .login-tab {
            flex: 1;
            padding: 0.6rem 0.5rem;
            border: none;
            border-radius: 60px;
            font-weight: 600;
            font-size: 0.85rem;
            cursor: pointer;
            transition: 0.25s ease;
            background: transparent;
            color: #4b6a89;
        }
        .login-tab.active {
            background: #0b3b5c;
            color: white;
            box-shadow: 0 4px 12px rgba(11, 59, 92, 0.2);
        }
        .login-tab:hover:not(.active) { background: rgba(11, 59, 92, 0.05); }

        .login-panel { display: none; }
        .login-panel.active { display: block; animation: fadeUp 0.3s ease; }

        .login-box .info-akun {
            background: #e9f2fa;
            border-radius: 12px;
            padding: 0.8rem 1rem;
            margin-bottom: 1.5rem;
            font-size: 0.8rem;
            color: #2d4b68;
        }
        .login-box .info-akun strong { color: #0b3b5c; }
        .login-box .form-group {
            margin-bottom: 1.2rem;
            text-align: left;
        }
        .login-box .form-group label {
            display: block;
            font-weight: 600;
            font-size: 0.9rem;
            color: #1e3a5f;
            margin-bottom: 0.3rem;
        }
        .login-box .form-group input,
        .login-box .form-group select {
            width: 100%;
            padding: 0.8rem 1rem;
            border: 2px solid #e9eef3;
            border-radius: 60px;
            font-size: 1rem;
            transition: 0.2s;
            outline: none;
            background: #f9fcff;
        }
        .login-box .form-group input:focus,
        .login-box .form-group select:focus {
            border-color: #0b3b5c;
            background: white;
            box-shadow: 0 0 0 4px rgba(11, 59, 92, 0.1);
        }
        .login-box .form-group input::placeholder { color: #b0c4d8; }
        .login-box .btn-login {
            width: 100%;
            padding: 0.9rem;
            background: #0b3b5c;
            color: white;
            border: none;
            border-radius: 60px;
            font-size: 1.1rem;
            font-weight: 600;
            cursor: pointer;
            transition: 0.25s;
            margin-top: 0.5rem;
        }
        .login-box .btn-login:hover {
            background: #1a4d6e;
            transform: translateY(-2px);
            box-shadow: 0 8px 20px rgba(11, 59, 92, 0.3);
        }
        .login-box .btn-login:active { transform: scale(0.97); }
        .login-box .btn-login i { margin-right: 0.5rem; }
        .login-box .btn-login.btn-guru { background: #c44545; }
        .login-box .btn-login.btn-guru:hover { background: #a33a3a; }
        .login-box .error-msg {
            color: #c44545;
            font-size: 0.85rem;
            margin-top: 0.8rem;
            display: none;
        }
        .login-box .error-msg.show { display: block; }
        .login-box .footer-text {
            margin-top: 1.5rem;
            font-size: 0.8rem;
            color: #7a94b0;
        }

        /* ===== MAIN CONTENT ===== */
        .container {
            max-width: 1200px;
            margin: 0 auto;
            background: white;
            border-radius: 2.5rem 2.5rem 2rem 2rem;
            box-shadow: 0 20px 40px -12px rgba(0, 20, 30, 0.25);
            overflow: hidden;
            padding: 1.8rem 2.5rem 3rem;
            transition: all 0.3s ease;
        }

        /* ===== NAVIGASI ===== */
        nav {
            display: flex;
            flex-wrap: wrap;
            align-items: center;
            justify-content: space-between;
            border-bottom: 2px solid #e9eef3;
            padding-bottom: 0.9rem;
            margin-bottom: 2rem;
            position: sticky;
            top: 0;
            background: white;
            z-index: 100;
            padding-top: 0.5rem;
            transition: all 0.3s ease;
        }
        .logo-area {
            display: flex;
            align-items: center;
            gap: 0.75rem;
        }
        .logo-icon-nav {
            background: #0b3b5c;
            color: white;
            width: 44px;
            height: 44px;
            border-radius: 16px;
            display: flex;
            align-items: center;
            justify-content: center;
            font-size: 1.8rem;
            font-weight: 700;
            box-shadow: 0 4px 8px rgba(11, 59, 92, 0.2);
        }
        .logo-text {
            font-size: 1.35rem;
            font-weight: 700;
            letter-spacing: -0.3px;
            color: #0b3b5c;
        }
        .logo-text small {
            font-weight: 400;
            font-size: 0.9rem;
            color: #4b6a89;
            display: block;
            margin-top: -4px;
        }
        .nav-links {
            display: flex;
            flex-wrap: wrap;
            gap: 0.2rem 1.2rem;
            font-weight: 600;
            font-size: 1rem;
        }
        .nav-links a {
            text-decoration: none;
            color: #1e3a5f;
            padding: 0.4rem 0.1rem;
            border-bottom: 3px solid transparent;
            transition: 0.2s;
            cursor: pointer;
        }
        .nav-links a:hover {
            border-bottom-color: #0b3b5c;
            color: #0b2b44;
        }
        .nav-links a.active {
            border-bottom-color: #0b3b5c;
            color: #0b2b44;
        }
        .nav-links a.tugas-link {
            position: relative;
        }
        .nav-links a.tugas-link .badge-count {
            background: #c44545;
            color: white;
            border-radius: 50%;
            padding: 0.1rem 0.5rem;
            font-size: 0.6rem;
            font-weight: 700;
            margin-left: 0.2rem;
            vertical-align: top;
        }

        .mode-toggle-wrapper {
            display: flex;
            align-items: center;
            gap: 0.5rem;
            margin-left: 0.5rem;
        }
        .mode-toggle-wrapper span {
            font-size: 0.75rem;
            color: #4b6a89;
            font-weight: 500;
        }
        .mode-toggle {
            background: #e9eef3;
            border: none;
            border-radius: 30px;
            padding: 0.4rem 1rem;
            font-size: 0.85rem;
            font-weight: 600;
            color: #1e3a5f;
            cursor: pointer;
            transition: 0.25s ease;
            display: flex;
            align-items: center;
            gap: 0.4rem;
        }
        .mode-toggle:hover {
            background: #d0dce8;
            transform: scale(1.02);
        }
        .mode-toggle i { font-size: 1rem; }
        body.mode-hp .mode-toggle {
            background: #0b3b5c;
            color: white;
        }
        body.mode-hp .mode-toggle:hover {
            background: #1a4d6e;
        }

        /* ===== BANNER ===== */
        .banner {
            background: linear-gradient(135deg, #e1edf7 0%, #d0e2f0 100%);
            border-radius: 2rem;
            padding: 2rem 2.5rem;
            margin-bottom: 2.8rem;
            display: flex;
            flex-wrap: wrap;
            justify-content: space-between;
            align-items: center;
            box-shadow: inset 0 1px 4px rgba(255,255,255,0.8);
            transition: all 0.3s ease;
        }
        .banner h1 {
            font-size: 2.1rem;
            font-weight: 700;
            color: #0a2a42;
        }
        .banner h1 span {
            background: #0b3b5c;
            color: white;
            padding: 0.1rem 1rem;
            border-radius: 60px;
            font-size: 1.6rem;
            display: inline-block;
            margin-left: 6px;
        }
        .banner p {
            font-size: 1.1rem;
            max-width: 550px;
            color: #1f3d57;
            margin-top: 0.3rem;
        }
        .banner .quote-icon {
            font-size: 2.2rem;
            background: #ffffffcc;
            padding: 0.3rem 1.2rem;
            border-radius: 60px;
            box-shadow: 0 4px 8px rgba(0,0,0,0.03);
            backdrop-filter: blur(2px);
            color: #0b3b5c;
        }

        /* ===== USER INFO ===== */
        .user-info {
            background: #e8f5e9;
            border-radius: 1rem;
            padding: 0.8rem 1.2rem;
            margin-bottom: 1.5rem;
            display: flex;
            align-items: center;
            justify-content: space-between;
            flex-wrap: wrap;
            gap: 0.5rem;
        }
        .user-info p {
            color: #2e7d32;
            font-size: 0.95rem;
        }
        .user-info .role-badge {
            background: #0b3b5c;
            color: white;
            padding: 0.2rem 0.8rem;
            border-radius: 30px;
            font-size: 0.7rem;
            font-weight: 600;
        }

        /* ===== KONTEN ===== */
        .content-section {
            display: block;
            margin-bottom: 2.5rem;
            scroll-margin-top: 100px;
            padding-top: 1rem;
        }
        .content-section h2 {
            font-size: 1.9rem;
            font-weight: 600;
            color: #0b2c44;
            border-left: 6px solid #0b3b5c;
            padding-left: 1rem;
            margin-bottom: 1.5rem;
        }

        /* ===== ANGGOTA ===== */
        .pengurus-grid {
            display: grid;
            grid-template-columns: repeat(auto-fit, minmax(250px, 1fr));
            gap: 1.5rem;
            margin-bottom: 2.5rem;
        }
        .pengurus-card {
            background: #f9fcff;
            border-radius: 1.8rem;
            padding: 1.8rem 1.5rem;
            border: 1px solid #eaf0f6;
            transition: 0.2s ease;
            text-align: center;
        }
        .pengurus-card:hover {
            transform: translateY(-4px);
            border-color: #b6cfe5;
            box-shadow: 0 12px 24px -12px rgba(11, 59, 92, 0.15);
        }
        .pengurus-card .icon {
            font-size: 2.5rem;
            color: #0b3b5c;
            background: #dbe8f3;
            padding: 0.6rem;
            border-radius: 60px;
            width: 70px;
            display: inline-block;
            margin-bottom: 0.8rem;
        }
        .pengurus-card .jabatan {
            font-size: 0.85rem;
            color: #4b6a89;
            font-weight: 600;
            text-transform: uppercase;
            letter-spacing: 0.5px;
        }
        .pengurus-card .nama {
            font-size: 1.2rem;
            font-weight: 700;
            color: #0a2a42;
            margin: 0.3rem 0;
        }
        .pengurus-card .sub-nama {
            font-size: 0.95rem;
            color: #3d5a78;
        }
        .anggota-list {
            display: grid;
            grid-template-columns: repeat(auto-fill, minmax(200px, 1fr));
            gap: 0.8rem;
            background: #fafdff;
            padding: 1.5rem;
            border-radius: 2rem;
            border: 1px solid #eaf0f6;
        }
        .anggota-item {
            padding: 0.6rem 1rem;
            background: white;
            border-radius: 60px;
            border: 1px solid #eaf0f6;
            font-size: 0.95rem;
            transition: 0.15s;
            display: flex;
            align-items: center;
            gap: 0.5rem;
        }
        .anggota-item:hover {
            background: #e9f2fa;
            border-color: #b6cfe5;
        }
        .anggota-item .no {
            font-weight: 700;
            color: #0b3b5c;
            min-width: 28px;
            font-size: 0.8rem;
        }
        .anggota-item .nama {
            color: #1e293b;
        }

        /* ===== JADWAL ===== */
        .schedule-header {
            background: #0b3b5c;
            color: white;
            padding: 1rem 1.5rem;
            border-radius: 16px 16px 0 0;
            margin-bottom: 0;
        }
        .schedule-header h3 {
            font-size: 1.2rem;
            font-weight: 600;
        }
        .schedule-header p {
            font-size: 0.9rem;
            opacity: 0.9;
            margin-top: 2px;
        }
        .schedule-table {
            width: 100%;
            border-collapse: collapse;
            background: #fafdff;
            border-radius: 0 0 24px 24px;
            overflow: hidden;
            box-shadow: 0 2px 10px rgba(0,0,0,0.02);
            margin-bottom: 2rem;
        }
        .schedule-table th {
            background: #1a4d6e;
            color: white;
            font-weight: 600;
            padding: 0.8rem 0.6rem;
            text-align: left;
            font-size: 0.95rem;
        }
        .schedule-table td {
            padding: 0.7rem 0.6rem;
            border-bottom: 1px solid #e5edf5;
            font-size: 0.92rem;
        }
        .schedule-table tr:last-child td {
            border-bottom: none;
        }
        .schedule-table tr:hover td {
            background: #f2f8fe;
        }
        .schedule-table .istirahat {
            background: #fff8e1;
            font-weight: 600;
            color: #b8860b;
        }
        .schedule-table .istirahat td {
            background: #fff8e1;
        }
        .schedule-table .pembiasaan {
            background: #e8f0fe;
            color: #1a3a5c;
        }
        .schedule-table .pembiasaan td {
            background: #e8f0fe;
        }
        .schedule-table .hari-label {
            background: #e9f2fa;
            font-weight: 700;
            font-size: 1rem;
        }
        .schedule-table .hari-label td {
            background: #dce8f3;
            font-weight: 700;
            font-size: 1rem;
            padding: 0.5rem 0.6rem;
        }
        .schedule-table .kode-ruang {
            font-weight: 600;
            color: #0b3b5c;
        }
        .schedule-table .guru {
            font-size: 0.85rem;
            color: #3d5a78;
        }

        /* ===== STATISTIK ===== */
        .stat-wrap {
            display: flex;
            flex-wrap: wrap;
            gap: 2rem 3.5rem;
            background: #edf5fc;
            padding: 1.8rem 2.5rem;
            border-radius: 2.5rem;
            justify-content: center;
        }
        .stat-item {
            text-align: center;
        }
        .stat-item .number {
            font-size: 2.8rem;
            font-weight: 700;
            color: #0a2e48;
        }
        .stat-item .label {
            font-size: 1rem;
            color: #2d4b68;
            letter-spacing: 0.3px;
        }

        /* ===== TUGAS SISWA ===== */
        .tugas-siswa-container {
            display: grid;
            grid-template-columns: repeat(auto-fill, minmax(350px, 1fr));
            gap: 1.2rem;
            margin-top: 1rem;
        }
        .tugas-siswa-card {
            background: white;
            border: 1px solid #eaf0f6;
            border-radius: 1.5rem;
            padding: 1.5rem;
            transition: 0.2s;
            box-shadow: 0 4px 12px rgba(0,0,0,0.02);
        }
        .tugas-siswa-card:hover {
            transform: translateY(-4px);
            border-color: #b6cfe5;
            box-shadow: 0 12px 24px -12px rgba(11, 59, 92, 0.15);
        }
        .tugas-siswa-card .guru-name {
            font-size: 0.8rem;
            color: #4b6a89;
            font-weight: 600;
            text-transform: uppercase;
            letter-spacing: 0.5px;
        }
        .tugas-siswa-card .judul {
            font-size: 1.1rem;
            font-weight: 700;
            color: #0a2a42;
            margin: 0.3rem 0;
        }
        .tugas-siswa-card .meta {
            font-size: 0.75rem;
            color: #7a94b0;
            margin-top: 0.5rem;
            display: flex;
            justify-content: space-between;
            flex-wrap: wrap;
        }
        .tugas-siswa-card .badge {
            display: inline-block;
            padding: 0.2rem 0.8rem;
            border-radius: 30px;
            font-size: 0.7rem;
            font-weight: 600;
        }
        .tugas-siswa-card .badge.ujian {
            background: #ffebee;
            color: #c62828;
        }
        .tugas-siswa-card .badge.tugas {
            background: #e8f5e9;
            color: #2e7d32;
        }
        .tugas-siswa-card textarea {
            width: 100%;
            padding: 0.5rem;
            border: 2px solid #e9eef3;
            border-radius: 12px;
            font-size: 0.9rem;
            resize: vertical;
            font-family: inherit;
        }
        .tugas-siswa-card textarea:focus {
            border-color: #0b3b5c;
            outline: none;
        }
        .tugas-siswa-card .btn-kirim {
            background: #0b3b5c;
            color: white;
            border: none;
            border-radius: 60px;
            padding: 0.5rem 1.5rem;
            cursor: pointer;
            font-weight: 600;
            transition: 0.2s;
        }
        .tugas-siswa-card .btn-kirim:hover {
            background: #1a4d6e;
            transform: translateY(-2px);
        }
        .tugas-siswa-card .status-sudah {
            color: #2e7d32;
            font-size: 0.85rem;
            font-weight: 600;
        }
        .tugas-siswa-card .info-tugas-lama {
            background: #fff3cd;
            border-radius: 8px;
            padding: 0.5rem;
            margin-top: 0.5rem;
            font-size: 0.85rem;
            color: #856404;
        }

        /* ===== FOOTER ===== */
        .footer-note {
            margin-top: 3rem;
            padding-top: 1.5rem;
            border-top: 2px dashed #cbdbe9;
            display: flex;
            flex-wrap: wrap;
            justify-content: space-between;
            align-items: center;
            color: #2a4c6a;
        }
        .footer-note .apps a {
            display: inline-block;
            background: #0b3b5c;
            color: white;
            padding: 0.3rem 1.3rem;
            border-radius: 60px;
            margin-left: 0.5rem;
            font-size: 0.9rem;
            font-weight: 500;
            text-decoration: none;
            transition: 0.15s;
        }
        .footer-note .apps a:hover {
            background: #1f5277;
        }
        .footer-note .emoji-text {
            font-size: 1.1rem;
        }

        /* ===== SCROLL TOP ===== */
        .scroll-top {
            position: fixed;
            bottom: 30px;
            right: 30px;
            background: #0b3b5c;
            color: white;
            width: 50px;
            height: 50px;
            border-radius: 50%;
            border: none;
            font-size: 1.5rem;
            cursor: pointer;
            box-shadow: 0 4px 15px rgba(11, 59, 92, 0.3);
            transition: 0.3s;
            display: none;
            z-index: 200;
        }
        .scroll-top:hover {
            transform: scale(1.1);
            background: #1a4d6e;
        }
        .scroll-top.show { display: block; }

        /* ===== GURU PANEL ===== */
        #guruPanel {
            display: none;
            max-width: 1200px;
            margin: 0 auto;
            padding: 2rem;
            background: white;
            border-radius: 2.5rem;
            box-shadow: 0 20px 40px -12px rgba(0,20,30,0.25);
        }
        #guruPanel .guru-header {
            display: flex;
            justify-content: space-between;
            align-items: center;
            border-bottom: 2px solid #e9eef3;
            padding-bottom: 1rem;
            margin-bottom: 2rem;
            flex-wrap: wrap;
            gap: 1rem;
        }
        #guruPanel .guru-header h2 {
            color: #0b3b5c;
        }
        #guruPanel .guru-header h2 i {
            color: #c44545;
        }
        #guruPanel .guru-badge {
            background: #e8f5e9;
            padding: 0.3rem 1rem;
            border-radius: 30px;
            font-size: 0.9rem;
            color: #2e7d32;
        }
        #guruPanel .guru-grid {
            display: grid;
            grid-template-columns: 1fr 1fr;
            gap: 2rem;
        }
        #guruPanel .guru-card {
            background: #f9fcff;
            border-radius: 1.5rem;
            padding: 1.5rem;
            border: 1px solid #eaf0f6;
        }
        #guruPanel .guru-card h3 {
            color: #0b3b5c;
            margin-bottom: 1rem;
        }
        #guruPanel .guru-card .btn-group {
            display: grid;
            grid-template-columns: 1fr 1fr;
            gap: 0.5rem;
            margin-bottom: 1rem;
        }
        #guruPanel .guru-card .btn-group button {
            padding: 0.6rem;
            border: none;
            border-radius: 12px;
            cursor: pointer;
            font-weight: 600;
            color: white;
        }
        #guruPanel .guru-card .btn-group .btn-tugas {
            background: #0b3b5c;
        }
        #guruPanel .guru-card .btn-group .btn-tugas:hover {
            background: #1a4d6e;
        }
        #guruPanel .guru-card .btn-group .btn-ujian {
            background: #c44545;
        }
        #guruPanel .guru-card .btn-group .btn-ujian:hover {
            background: #a33a3a;
        }

        .tugas-item-guru {
            background: white;
            border: 1px solid #eaf0f6;
            border-radius: 1rem;
            padding: 0.8rem 1rem;
            margin-bottom: 0.5rem;
            display: flex;
            justify-content: space-between;
            align-items: center;
            flex-wrap: wrap;
            gap: 0.5rem;
        }
        .tugas-item-guru .info strong {
            color: #0b3b5c;
        }
        .tugas-item-guru .info .meta {
            font-size: 0.8rem;
            color: #7a94b0;
        }
        .tugas-item-guru .badge {
            display: inline-block;
            padding: 0.1rem 0.6rem;
            border-radius: 30px;
            font-size: 0.7rem;
            font-weight: 600;
            margin-left: 0.5rem;
        }
        .tugas-item-guru .badge.ujian {
            background: #ffebee;
            color: #c62828;
        }
        .tugas-item-guru .badge.tugas {
            background: #e8f5e9;
            color: #2e7d32;
        }
        .tugas-item-guru .actions button {
            background: #c44545;
            color: white;
            border: none;
            border-radius: 30px;
            padding: 0.2rem 0.8rem;
            cursor: pointer;
            font-size: 0.8rem;
        }
        .tugas-item-guru .actions button:hover {
            background: #a33a3a;
        }

        .siswa-item-guru {
            display: flex;
            flex-direction: column;
            align-items: stretch;
            gap: 0.3rem;
            padding: 0.6rem 0.8rem;
            background: white;
            border-radius: 12px;
            border: 1px solid #eaf0f6;
            margin-bottom: 0.5rem;
        }
        .siswa-item-guru .row {
            display: flex;
            justify-content: space-between;
            align-items: center;
            flex-wrap: wrap;
            gap: 0.3rem;
        }
        .siswa-item-guru .status {
            font-size: 0.75rem;
            padding: 0.1rem 0.6rem;
            border-radius: 30px;
        }
        .siswa-item-guru .status.sudah {
            background: #e8f5e9;
            color: #2e7d32;
        }
        .siswa-item-guru .status.belum {
            background: #ffebee;
            color: #c62828;
        }
        .siswa-item-guru input[type="number"] {
            width: 55px;
            padding: 0.2rem 0.4rem;
            border: 2px solid #e9eef3;
            border-radius: 30px;
            font-size: 0.85rem;
            text-align: center;
        }
        .siswa-item-guru input[type="number"]:focus {
            border-color: #0b3b5c;
            outline: none;
        }
        .siswa-item-guru .jawaban-box {
            background: #f4faff;
            border-radius: 8px;
            padding: 0.5rem;
            margin-top: 0.3rem;
            font-size: 0.85rem;
        }
        .siswa-item-guru .jawaban-box .jawaban-item {
            margin-top: 0.2rem;
        }
        .siswa-item-guru .jawaban-box .jawaban-item .q {
            font-weight: 600;
        }
        .siswa-item-guru .jawaban-box .jawaban-item .a {
            color: #1e293b;
        }
        .siswa-item-guru .jawaban-box .waktu {
            font-size: 0.7rem;
            color: #7a94b0;
            margin-top: 0.2rem;
        }
        .siswa-item-guru .tidak-ada-jawaban {
            color: #7a94b0;
            font-size: 0.85rem;
            font-style: italic;
        }

        #guruPanel select {
            width: 100%;
            padding: 0.6rem 1rem;
            border: 2px solid #e9eef3;
            border-radius: 12px;
            font-size: 1rem;
            background: white;
            margin-bottom: 1rem;
        }
        #guruPanel select:focus {
            border-color: #0b3b5c;
            outline: none;
        }

        /* ===== HIDDEN ===== */
        #mainContent { display: none; }
        #mainContent.show { display: block; }
        .login-page { display: flex; }
        .login-page.hidden { display: none; }

        /* ===== TUGAS PAGE ===== */
        .tugas-page {
            display: none;
            padding: 2rem 0;
        }
        .tugas-page.active {
            display: block;
            animation: fadeUp 0.3s ease;
        }

        /* ===== FIREBASE STATUS ===== */
        .firebase-status {
            display: inline-block;
            padding: 0.2rem 0.8rem;
            border-radius: 30px;
            font-size: 0.7rem;
            font-weight: 600;
        }
        .firebase-status.online {
            background: #e8f5e9;
            color: #2e7d32;
        }
        .firebase-status.offline {
            background: #ffebee;
            color: #c62828;
        }

        /* ===== MODE HP ===== */
        body.mode-hp {
            padding: 0.5rem;
            background: #e8edf3;
        }
        body.mode-hp .container {
            padding: 1rem 1.2rem 2rem;
            border-radius: 1.5rem;
            max-width: 480px;
            margin: 0 auto;
            box-shadow: 0 10px 30px -8px rgba(0, 20, 30, 0.3);
        }
        body.mode-hp nav {
            flex-direction: column;
            align-items: stretch;
            gap: 0.5rem;
            padding-bottom: 0.6rem;
        }
        body.mode-hp .logo-area { justify-content: center; }
        body.mode-hp .logo-text { font-size: 1.1rem; }
        body.mode-hp .logo-text small { font-size: 0.75rem; }
        body.mode-hp .logo-icon-nav { width: 38px; height: 38px; font-size: 1.4rem; }
        body.mode-hp .nav-links { justify-content: center; gap: 0.2rem 0.7rem; font-size: 0.85rem; flex-wrap: wrap; }
        body.mode-hp .nav-links a { padding: 0.3rem 0.5rem; font-size: 0.8rem; }
        body.mode-hp .banner { padding: 1.2rem 1.2rem; flex-direction: column; align-items: start; gap: 0.8rem; }
        body.mode-hp .banner h1 { font-size: 1.6rem; }
        body.mode-hp .banner h1 span { font-size: 1.3rem; padding: 0.05rem 0.8rem; }
        body.mode-hp .banner p { font-size: 0.95rem; }
        body.mode-hp .banner .quote-icon { font-size: 1.5rem; padding: 0.2rem 0.8rem; }
        body.mode-hp .content-section h2 { font-size: 1.4rem; padding-left: 0.7rem; }
        body.mode-hp .pengurus-grid { grid-template-columns: 1fr 1fr; gap: 0.8rem; }
        body.mode-hp .pengurus-card { padding: 1rem 0.8rem; border-radius: 1.2rem; }
        body.mode-hp .pengurus-card .icon { font-size: 1.8rem; width: 50px; padding: 0.4rem; }
        body.mode-hp .pengurus-card .nama { font-size: 1rem; }
        body.mode-hp .pengurus-card .sub-nama { font-size: 0.8rem; }
        body.mode-hp .pengurus-card .jabatan { font-size: 0.7rem; }
        body.mode-hp .anggota-list { grid-template-columns: 1fr 1fr; gap: 0.5rem; padding: 0.8rem; }
        body.mode-hp .anggota-item { font-size: 0.8rem; padding: 0.4rem 0.6rem; }
        body.mode-hp .anggota-item .no { min-width: 22px; font-size: 0.7rem; }
        body.mode-hp .schedule-table th,
        body.mode-hp .schedule-table td { font-size: 0.7rem; padding: 0.3rem 0.2rem; }
        body.mode-hp .schedule-table .guru { font-size: 0.65rem; }
        body.mode-hp .schedule-header h3 { font-size: 1rem; }
        body.mode-hp .schedule-header p { font-size: 0.75rem; }
        body.mode-hp .stat-wrap { padding: 1rem 1.2rem; gap: 1rem 1.5rem; }
        body.mode-hp .stat-item .number { font-size: 1.8rem; }
        body.mode-hp .stat-item .label { font-size: 0.8rem; }
        body.mode-hp .footer-note { flex-direction: column; gap: 0.8rem; text-align: center; font-size: 0.9rem; }
        body.mode-hp .footer-note .apps a { font-size: 0.75rem; padding: 0.2rem 0.8rem; margin: 0 0.2rem; }
        body.mode-hp .scroll-top { width: 40px; height: 40px; font-size: 1.2rem; bottom: 15px; right: 15px; }
        body.mode-hp .mode-toggle { padding: 0.3rem 0.8rem; font-size: 0.75rem; }
        body.mode-hp .login-box { padding: 2rem 1.5rem; border-radius: 1.8rem; }
        body.mode-hp .login-box h1 { font-size: 1.5rem; }
        body.mode-hp .login-box .logo-icon { width: 60px; height: 60px; font-size: 2rem; }
        body.mode-hp .login-box .form-group input { padding: 0.7rem 0.9rem; font-size: 0.9rem; }
        body.mode-hp .login-box .btn-login { padding: 0.7rem; font-size: 1rem; }
        body.mode-hp .login-box .info-akun { font-size: 0.7rem; padding: 0.6rem 0.8rem; }
        body.mode-hp .loading-title { font-size: 2rem; }
        body.mode-hp .loading-sub { font-size: 0.8rem; }
        body.mode-hp .loading-desc { font-size: 0.8rem; }
        body.mode-hp .loading-footer { font-size: 0.7rem; }
        body.mode-hp .loader-wrapper { width: 55px; height: 55px; }
        body.mode-hp .loader { width: 55px; height: 55px; }
        body.mode-hp .loader-wrapper::before { top: -4px; left: -4px; right: -4px; bottom: -4px; }
        body.mode-hp .login-tab { font-size: 0.7rem; padding: 0.4rem 0.3rem; }
        body.mode-hp .user-info { font-size: 0.85rem; padding: 0.5rem 0.8rem; }
        body.mode-hp .tugas-siswa-container { grid-template-columns: 1fr; }
        body.mode-hp .siswa-item-guru .row { flex-direction: column; align-items: stretch; gap: 0.3rem; }
        @media (max-width: 400px) {
            body.mode-hp .pengurus-grid { grid-template-columns: 1fr; }
            body.mode-hp .anggota-list { grid-template-columns: 1fr; }
        }
        @media (max-width: 768px) {
            #guruPanel .guru-grid { grid-template-columns: 1fr; }
        }
        @media (max-width: 650px) {
            body:not(.mode-hp) .container { padding: 1.2rem; }
            body:not(.mode-hp) nav { flex-direction: column; align-items: start; gap: 0.8rem; }
            body:not(.mode-hp) .nav-links { gap: 0.3rem 1rem; }
            body:not(.mode-hp) .banner { flex-direction: column; align-items: start; gap: 1rem; }
            body:not(.mode-hp) .banner h1 { font-size: 1.8rem; }
            body:not(.mode-hp) .stat-wrap { flex-direction: column; align-items: center; gap: 1.2rem; }
            body:not(.mode-hp) .pengurus-grid { grid-template-columns: 1fr; }
            body:not(.mode-hp) .anggota-list { grid-template-columns: 1fr; }
            body:not(.mode-hp) .tugas-siswa-container { grid-template-columns: 1fr; }
        }
    </style>
</head>
<body>

<!-- ====== LOADING ====== -->
<div id="loadingOverlay">
    <div class="loading-container">
        <div class="loading-title"><span class="highlight">WEBSITE XII TJKT 3</span></div>
        <div class="loading-sub">SMK Pelita Ciampea</div>
        <div class="loading-divider"></div>
        <div class="loading-desc">Memuat sistem informasi kelas...</div>
        <div class="loader-wrapper"><div class="loader"></div></div>
        <div class="loader-text">Loading<span>.</span><span>.</span><span>.</span></div>
        <div class="loading-footer"><strong>@DitzProject</strong></div>
    </div>
</div>

<!-- ====== LOGIN ====== -->
<div class="login-page" id="loginPage">
    <div class="login-box">
        <div class="logo-icon">XII</div>
        <h1>XII TJKT 3</h1>
        <div class="subtitle">SMK Pelita Ciampea <div class="by">by <strong>DitzProject</strong></div></div>

        <div class="login-tabs">
            <button class="login-tab active" onclick="switchLoginTab('siswa')"><i class="fas fa-user-graduate"></i> Siswa</button>
            <button class="login-tab" onclick="switchLoginTab('guru')"><i class="fas fa-chalkboard-teacher"></i> Guru</button>
        </div>

        <!-- PANEL SISWA -->
        <div class="login-panel active" id="panelSiswa">
            <div class="info-akun"><i class="fas fa-info-circle"></i> Gunakan <strong>Nama Siswa</strong> sebagai Username<br>Password: <strong>xii tjkt 3</strong></div>
            <form id="loginFormSiswa" onsubmit="return handleLoginSiswa(event)">
                <div class="form-group">
                    <label><i class="fas fa-user"></i> Username (Nama Siswa)</label>
                    <input type="text" id="username" placeholder="Masukkan nama siswa" required />
                </div>
                <div class="form-group">
                    <label><i class="fas fa-lock"></i> Password</label>
                    <input type="password" id="password" placeholder="Masukkan password" required />
                </div>
                <button type="submit" class="btn-login"><i class="fas fa-sign-in-alt"></i> Login sebagai Siswa</button>
                <div class="error-msg" id="errorMsgSiswa"><i class="fas fa-exclamation-circle"></i> Username atau password salah!</div>
            </form>
            <div class="footer-text">Contoh: <strong>Ahyan Fardhin Murtaza</strong> / <strong>xii tjkt 3</strong></div>
        </div>

        <!-- PANEL GURU -->
        <div class="login-panel" id="panelGuru">
            <div class="info-akun"><i class="fas fa-info-circle"></i> Pilih nama guru dan masukkan password<br>Password: <strong>G7!mQ#2vL9@xR4pK</strong></div>
            <form id="loginFormGuru" onsubmit="return handleLoginGuru(event)">
                <div class="form-group">
                    <label><i class="fas fa-user-tie"></i> Pilih Guru</label>
                    <select id="guruLoginSelect" required>
                        <option value="">-- Pilih Guru --</option>
                        <option value="M. Nugraha">M. Nugraha</option>
                        <option value="Mr. Tony">Mr. Tony</option>
                        <option value="Ridwan Agus Tesyah">Ridwan Agus Tesyah</option>
                        <option value="Muhamad Khoerobi">Muhamad Khoerobi</option>
                        <option value="Opah Nasopah">Opah Nasopah</option>
                        <option value="Zulius Remit">Zulius Remit</option>
                        <option value="Ferry Lesmana">Ferry Lesmana</option>
                        <option value="Sidik">Sidik</option>
                        <option value="Selvi Sukaesih">Selvi Sukaesih</option>
                        <option value="Doni Budiyanto Badru Tamam">Doni Budiyanto Badru Tamam</option>
                    </select>
                </div>
                <div class="form-group">
                    <label><i class="fas fa-lock"></i> Password</label>
                    <input type="password" id="guruLoginPassword" placeholder="Masukkan password guru" required />
                </div>
                <button type="submit" class="btn-login btn-guru"><i class="fas fa-sign-in-alt"></i> Login sebagai Guru</button>
                <div class="error-msg" id="errorMsgGuru"><i class="fas fa-exclamation-circle"></i> Password salah!</div>
            </form>
            <div class="footer-text">Semua guru menggunakan password yang sama</div>
        </div>
    </div>
</div>

<!-- ====== MAIN CONTENT ====== -->
<div id="mainContent">
    <div class="container">

        <!-- ====== NAVIGASI ====== -->
        <nav id="navbar">
            <div class="logo-area">
                <div class="logo-icon-nav">XII</div>
                <div class="logo-text">TJKT 3 <small>Kelas XII TJKT 3</small></div>
            </div>
            <div style="display: flex; flex-wrap: wrap; align-items: center; gap: 0.5rem;">
                <div class="nav-links">
                    <a href="#beranda" class="active" data-section="beranda">Beranda</a>
                    <a href="#tentang" data-section="tentang">Tentang</a>
                    <a href="#anggota" data-section="anggota">Anggota</a>
                    <a href="#jadwal" data-section="jadwal">Jadwal Pelajaran</a>
                    <a href="#statistik" data-section="statistik">Statistik Anggota</a>
                    <a href="#tugas" class="tugas-link" id="tugasNav" data-section="tugas">Tugas/Ujian <span class="badge-count" id="tugasBadge">0</span></a>
                </div>
                <div class="mode-toggle-wrapper">
                    <span><i class="fas fa-mobile-alt"></i></span>
                    <button class="mode-toggle" id="modeToggle" onclick="toggleMode()"><i class="fas fa-exchange-alt"></i> HP</button>
                    <span><i class="fas fa-desktop"></i></span>
                </div>
                <button onclick="logout()" id="logoutBtn" style="background:#c44545;color:white;border:none;border-radius:30px;padding:0.4rem 1rem;font-weight:600;cursor:pointer;font-size:0.85rem;"><i class="fas fa-sign-out-alt"></i> Logout</button>
                <button onclick="toggleGuruMode()" id="btnGuruNav" style="display:none;background:#0b3b5c;color:white;border:none;border-radius:30px;padding:0.4rem 1rem;font-weight:600;cursor:pointer;font-size:0.85rem;">
                    <i class="fas fa-chalkboard-teacher"></i> Guru
                </button>
            </div>
        </nav>

        <!-- ====== BANNER ====== -->
        <div class="banner" id="mainBanner">
            <div>
                <h1><span>XII</span> TJKT 3</h1>
                <p><i class="fas fa-school" style="margin-right: 6px;"></i> SMK Pelita Ciampea · T.P 2026/2027
                    <span class="firebase-status online" id="firebaseStatus"><i class="fas fa-circle" style="font-size:0.5rem;"></i> Firebase Online</span>
                </p>
                <p style="margin-top: 6px; font-size: 0.95rem; opacity: 0.8;"><i class="fas fa-globe"></i> “F V : kelas lu punya web sendiri 😊😊😊”</p>
            </div>
            <div class="quote-icon"><i class="fas fa-laptop-code"></i> #DitzProject</div>
        </div>

        <!-- ====== USER INFO ====== -->
        <div class="user-info">
            <p><i class="fas fa-user-check"></i> Login sebagai: <strong id="displayNamaSiswa">-</strong></p>
            <span class="role-badge"><i class="fas fa-user-graduate"></i> Siswa</span>
        </div>

        <!-- ====== BERANDA ====== -->
        <section class="content-section" id="beranda">
            <h2><i class="fas fa-home" style="margin-right: 10px;"></i> Beranda</h2>
            <p style="font-size: 1.1rem; max-width: 750px; margin-bottom: 1.5rem;">
                Selamat datang di Website kelas <strong>XII TJKT 3 - SMK Pelita Ciampea</strong>. 
                Di sini kami mengelola informasi anggota, jadwal pelajaran, dan perkembangan 
                kelas secara transparan. Web ini adalah wujud nyata dari semangat kolaborasi.
            </p>
            <div style="display: flex; flex-wrap: wrap; gap: 1rem; background: #e9f2fa; padding: 1.2rem 1.8rem; border-radius: 60px;">
                <span><i class="fas fa-calendar-check"></i> 6 Hari Pelajaran</span>
                <span><i class="fas fa-user-graduate"></i> 38 Anggota</span>
                <span><i class="fas fa-chalkboard-teacher"></i> 10 Guru</span>
            </div>
        </section>

        <!-- ====== TENTANG ====== -->
        <section class="content-section" id="tentang">
            <h2><i class="fas fa-info-circle" style="margin-right: 10px;"></i> Tentang</h2>
            <div style="background: #f4faff; padding: 1.5rem 2rem; border-radius: 2rem;">
                <p style="font-size: 1.05rem;"><strong>Website ini bertujuan</strong> untuk memberitahukan bahwa XII TJKT 3 itu adalah kelas unggulan. Website ini juga menyediakan data kelas</p>
                <p style="margin-top: 0.5rem;">Kelas <strong>XII TJKT 3</strong> adalah bagian dari SMK Pelita Ciampea dengan program keahlian Teknik Jaringan Komputer dan Telekomunikasi. Tahun pelajaran 2026/2027 (Revisi 0).</p>
                <div style="display: flex; gap: 1.2rem; flex-wrap: wrap; margin-top: 1rem;">
                    <span><i class="fas fa-check-circle" style="color: #0b3b5c;"></i> Visi: Menjadi kelas unggulan di bidang TJKT</span>
                    <span><i class="fas fa-check-circle" style="color: #0b3b5c;"></i> Misi: Kolaborasi, Inovasi, Prestasi</span>
                </div>
            </div>
        </section>

        <!-- ====== ANGGOTA ====== -->
        <section class="content-section" id="anggota">
            <h2><i class="fas fa-user-friends" style="margin-right: 10px;"></i> Anggota Kelas XII TJKT 3</h2>
            
            <h3 style="margin-bottom: 1rem; color: #0b3b5c;"><i class="fas fa-crown"></i> Pengurus Kelas</h3>
            <div class="pengurus-grid">
                <div class="pengurus-card"><div class="icon"><i class="fas fa-user-tie"></i></div><div class="jabatan">Ketua Kelas</div><div class="nama">Dias Irpan Gunawan</div></div>
                <div class="pengurus-card"><div class="icon"><i class="fas fa-user-graduate"></i></div><div class="jabatan">Wakil Ketua</div><div class="nama">Asean Gabriel</div></div>
                <div class="pengurus-card"><div class="icon"><i class="fas fa-user-cog"></i></div><div class="jabatan">Sekretaris</div><div class="nama">Ince Unka Sultan Azir</div><div class="sub-nama">Siti Amelia</div></div>
                <div class="pengurus-card"><div class="icon"><i class="fas fa-user-astronaut"></i></div><div class="jabatan">Bendahara</div><div class="nama">Idgi Yudistira Arfi</div><div class="sub-nama">Fabian Elpasya Bintang</div></div>
            </div>

            <h3 style="margin-bottom: 1rem; color: #0b3b5c;"><i class="fas fa-users"></i> Daftar Anggota (38 Siswa)</h3>
            <div class="anggota-list" id="anggotaContainer"></div>
            <p style="margin-top: 1rem; font-size: 0.9rem; color: #4b6a89;"><i class="fas fa-info-circle"></i> Total: <span id="totalAnggota">38</span> siswa terdaftar</p>
        </section>

        <!-- ====== JADWAL ====== -->
        <section class="content-section" id="jadwal">
            <h2><i class="fas fa-clock" style="margin-right: 10px;"></i> Jadwal Pelajaran</h2>
            
            <div class="schedule-header">
                <h3>📚 JADWAL PELAJARAN KELAS XII TJKT 3</h3>
                <p>SMK Pelita Ciampea T.P 2026/2027 (Revisi 0)</p>
            </div>

            <table class="schedule-table"><thead><tr><th style="width:15%;">Jam</th><th style="width:20%;">Kode/Ruang</th><th style="width:40%;">Mata Pelajaran / Kegiatan</th><th style="width:25%;">Guru</th></tr></thead><tbody id="jadwalSenin"></tbody></table>
            <table class="schedule-table"><tbody id="jadwalSelasa"></tbody></table>
            <table class="schedule-table"><tbody id="jadwalRabu"></tbody></table>
            <table class="schedule-table"><tbody id="jadwalKamis"></tbody></table>
            <table class="schedule-table"><tbody id="jadwalJumat"></tbody></table>
            <table class="schedule-table"><tbody id="jadwalSabtu"></tbody></table>
        </section>

        <!-- ====== STATISTIK ====== -->
        <section class="content-section" id="statistik">
            <h2><i class="fas fa-chart-simple" style="margin-right: 10px;"></i> Statistik Anggota</h2>
            <div class="stat-wrap">
                <div class="stat-item"><div class="number" id="statTotal">38</div><div class="label">Total Siswa</div></div>
                <div class="stat-item"><div class="number" id="statLaki">18</div><div class="label">Laki-laki</div></div>
                <div class="stat-item"><div class="number" id="statPerempuan">20</div><div class="label">Perempuan</div></div>
                <div class="stat-item"><div class="number" id="statGuru">10</div><div class="label">Guru</div></div>
                <div class="stat-item"><div class="number" id="statHari">6</div><div class="label">Hari Efektif</div></div>
            </div>
        </section>

        <!-- ====== TUGAS SISWA ====== -->
        <section class="content-section" id="tugas" style="display:none;">
            <div class="tugas-page active">
                <h2><i class="fas fa-tasks" style="margin-right: 10px;"></i> Tugas & Ujian Harian</h2>
                <p style="font-size: 1rem; color: #4b6a89; margin-bottom: 1rem;">Daftar tugas dan ujian dari semua guru</p>
                <div id="tugasSiswaContainer"><p style="color: #7a94b0; text-align: center; padding: 2rem;"><i class="fas fa-spinner fa-spin"></i> Memuat tugas...</p></div>
            </div>
        </section>

        <!-- ====== FOOTER ====== -->
        <div class="footer-note">
            <div class="emoji-text"><i class="fas fa-heart" style="color: #c44545;"></i> <span style="font-weight: 500;">F V : kelas lu punya web sendiri 😊😊😊</span></div>
            <div class="apps">
                <a href="https://play.google.com/store/apps/details?id=com.kamal.adalah.software" target="_blank"><i class="fab fa-google-play"></i> App Store</a>
                <a href="https://play.google.com/store/apps/details?id=com.kamal.adalah.shop" target="_blank"><i class="fas fa-download"></i> Download Store</a>
            </div>
        </div>

    </div>
</div>

<!-- ====== GURU PANEL ====== -->
<div id="guruPanel">
    <div class="guru-header">
        <h2><i class="fas fa-chalkboard-teacher"></i> Dashboard Guru</h2>
        <div>
            <span class="guru-badge" id="guruNameBadge"><i class="fas fa-user-check"></i> -</span>
            <button onclick="logoutGuru()" style="background:#c44545;color:white;border:none;border-radius:30px;padding:0.4rem 1.2rem;cursor:pointer;margin-left:1rem;font-weight:600;"><i class="fas fa-sign-out-alt"></i> Keluar</button>
        </div>
    </div>
    
    <div class="guru-grid">
        <!-- Kolom Kiri: Kelola Tugas -->
        <div class="guru-card">
            <h3><i class="fas fa-list"></i> Kelola Tugas/Ujian</h3>
            <div class="btn-group">
                <button class="btn-tugas" onclick="buatTugas()"><i class="fas fa-plus-circle"></i> Tugas</button>
                <button class="btn-ujian" onclick="buatUjian()"><i class="fas fa-plus-circle"></i> Ujian</button>
            </div>
            <div id="guruTugasList">
                <p style="color:#7a94b0;text-align:center;padding:1rem;">Belum ada tugas</p>
            </div>
        </div>
        
        <!-- Kolom Kanan: Siswa & Nilai -->
        <div class="guru-card">
            <h3><i class="fas fa-users-check"></i> Siswa & Nilai</h3>
            <div style="margin-bottom:1rem;">
                <label style="font-weight:600;font-size:0.9rem;color:#1e3a5f;">Pilih Tugas:</label>
                <select id="guruPilihTugas" onchange="loadSiswaNilaiGuru()">
                    <option value="">-- Pilih tugas --</option>
                </select>
            </div>
            <div id="guruSiswaNilai">
                <p style="color:#7a94b0;text-align:center;padding:1rem;">Pilih tugas untuk melihat siswa</p>
            </div>
        </div>
    </div>
</div>

<!-- ====== SCROLL TOP ====== -->
<button class="scroll-top" id="scrollTopBtn" onclick="scrollToTop()"><i class="fas fa-arrow-up"></i></button>

<!-- ====== FIREBASE SDK ====== -->
<script src="https://www.gstatic.com/firebasejs/9.23.0/firebase-app-compat.js"></script>
<script src="https://www.gstatic.com/firebasejs/9.23.0/firebase-database-compat.js"></script>

<script>
    // ===== KONFIGURASI FIREBASE =====
    const firebaseConfig = {
        apiKey: "AIzaSyDxiDzk0n4VB9AqqT49sJxn8NDY3ktOf2s",
        authDomain: "xii-tjkt-3.firebaseapp.com",
        databaseURL: "https://xii-tjkt-3-default-rtdb.asia-southeast1.firebasedatabase.app",
        projectId: "xii-tjkt-3",
        storageBucket: "xii-tjkt-3.firebasestorage.app",
        messagingSenderId: "241956035753",
        appId: "1:241956035753:web:c712603315b474c5909db8",
        measurementId: "G-0M226Y1LLJ"
    };

    // ===== INISIALISASI FIREBASE =====
    firebase.initializeApp(firebaseConfig);
    const database = firebase.database();

    console.log("🔥 Firebase Connected!");
    console.log("📡 Database:", firebaseConfig.databaseURL);

    // ===== UPDATE STATUS FIREBASE =====
    const statusEl = document.getElementById('firebaseStatus');
    if (statusEl) {
        statusEl.innerHTML = '<i class="fas fa-circle" style="font-size:0.5rem;"></i> Firebase Online';
        statusEl.className = 'firebase-status online';
    }

    // ====== DATA SISWA =====
    const daftarSiswa = [
        'Ahyan Fardhin Murtaza','Alfin Sandi Pratama','Aris Ariansyah W','Arya Rizky',
        'Aulia Syifa Rahmatia','Aura Febria Rizal','Bima Almughni Syahputra','Delan Akhdar Octora',
        'Ega Asegaf','Fabian Elpasya Bintang','Fikri Al Fathin','Idgi Yudistira Arfi',
        'Ince Unka Sultan Azir','Khailla Bilqis','M. Fikri','M. Akbar Rohim',
        'M. Rizky Fathurrachman','Maulana Ainuril Yakin','Moch Naufal Zaelani','Muhamad Aidil Akbar',
        'Muhamad Dafa Firdaus','Muhamad Fadli Alfansuri','Muhamad Fahreza','Muhamad Farhan Adzani',
        'Muhamad Fathir Awaludin','Muhamad Nur Fauzan','Muhamad Ridwan','Muhamad Yandi Yansyah Hakim',
        'Muhammad Haikal Pratama','Muhammad Rizki Nugraha','Muhammad Rizwan','Raditya Pradana',
        'Ramji Nugraha','Rendi Firdaus','Rey Marvel Horas','Satria Maulana',
        'Siti Amelia','Asean Gabriel'
    ];

    const daftarGuru = {
        'M. Nugraha':'G7!mQ#2vL9@xR4pK','Mr. Tony':'G7!mQ#2vL9@xR4pK',
        'Ridwan Agus Tesyah':'G7!mQ#2vL9@xR4pK','Muhamad Khoerobi':'G7!mQ#2vL9@xR4pK',
        'Opah Nasopah':'G7!mQ#2vL9@xR4pK','Zulius Remit':'G7!mQ#2vL9@xR4pK',
        'Ferry Lesmana':'G7!mQ#2vL9@xR4pK','Sidik':'G7!mQ#2vL9@xR4pK',
        'Selvi Sukaesih':'G7!mQ#2vL9@xR4pK','Doni Budiyanto Badru Tamam':'G7!mQ#2vL9@xR4pK'
    };

    let guruLogin = null;
    let isGuruMode = false;

    // ===== RENDER ANGGOTA =====
    function renderAnggota() {
        const container = document.getElementById('anggotaContainer');
        container.innerHTML = '';
        daftarSiswa.forEach((nama, i) => {
            const item = document.createElement('div');
            item.className = 'anggota-item';
            item.innerHTML = `<span class="no">${i+1}.</span><span class="nama">${nama}</span>`;
            container.appendChild(item);
        });
        document.getElementById('totalAnggota').textContent = daftarSiswa.length;
    }

    // ===== RENDER JADWAL =====
    const jadwalData = {
        senin: [
            {jam:"06.30 – 07.00",ruang:"-",mapel:"Kegiatan Pembiasaan",guru:"-"},
            {jam:"07.00 – 07.40",ruang:"-",mapel:"Upacara Pengibaran Bendera",guru:"-"},
            {jam:"07.40 – 08.20",ruang:"31",mapel:"Produktif TJKT",guru:"Pak M. NUGRAHA, S.Kom"},
            {jam:"08.20 – 09.00",ruang:"31",mapel:"Produktif TJKT",guru:"Pak M. NUGRAHA, S.Kom"},
            {jam:"09.00 – 09.40",ruang:"29",mapel:"Bahasa Indonesia",guru:"Pak M. TONY, S.S"},
            {jam:"09.40 – 10.00",ruang:"-",mapel:"🟡 ISTIRAHAT",guru:"-"},
            {jam:"10.00 – 10.30",ruang:"31",mapel:"Produktif TJKT",guru:"Pak M. NUGRAHA, S.Kom"},
            {jam:"10.30 – 11.10",ruang:"31",mapel:"Produktif TJKT",guru:"Pak M. NUGRAHA, S.Kom"},
            {jam:"11.10 – 11.50",ruang:"31",mapel:"Produktif TJKT",guru:"Pak M. NUGRAHA, S.Kom"},
            {jam:"11.50 – 12.30",ruang:"31",mapel:"Produktif TJKT",guru:"Pak M. NUGRAHA, S.Kom"}
        ],
        selasa: [
            {jam:"06.30 – 07.00",ruang:"-",mapel:"Kegiatan Pembiasaan",guru:"-"},
            {jam:"07.00 – 07.40",ruang:"303",mapel:"Pendidikan Agama",guru:"Pak RIDWAN AGUS TESYAH, S.E"},
            {jam:"07.40 – 08.20",ruang:"303",mapel:"Pendidikan Agama",guru:"Pak RIDWAN AGUS TESYAH, S.E"},
            {jam:"08.20 – 09.00",ruang:"248",mapel:"Matematika",guru:"Pak MUHAMAD KHOEROBI, A.Md"},
            {jam:"09.00 – 09.40",ruang:"248",mapel:"Matematika",guru:"Pak MUHAMAD KHOEROBI, A.Md"},
            {jam:"09.40 – 10.00",ruang:"-",mapel:"🟡 ISTIRAHAT",guru:"-"},
            {jam:"10.00 – 10.30",ruang:"57",mapel:"Bahasa Inggris",guru:"Bu OPAH NASOPAH, S.E"},
            {jam:"10.30 – 11.10",ruang:"151",mapel:"PJOK",guru:"Pak ZULIUS REMIT, S.Pd"},
            {jam:"11.10 – 11.50",ruang:"237",mapel:"Produktif TKJ",guru:"Pak FERRY LESMANA, S.Kom"},
            {jam:"11.50 – 12.30",ruang:"237",mapel:"Produktif TKJ",guru:"Pak FERRY LESMANA, S.Kom"}
        ],
        rabu: [
            {jam:"06.30 – 07.00",ruang:"-",mapel:"Kegiatan Pembiasaan",guru:"-"},
            {jam:"07.00 – 07.40",ruang:"237",mapel:"Produktif TKJ",guru:"Pak FERRY LESMANA, S.Kom"},
            {jam:"07.40 – 08.20",ruang:"237",mapel:"Produktif TKJ",guru:"Pak FERRY LESMANA, S.Kom"},
            {jam:"08.20 – 09.00",ruang:"237",mapel:"Produktif TKJ",guru:"Pak FERRY LESMANA, S.Kom"},
            {jam:"09.00 – 09.40",ruang:"16",mapel:"PKN",guru:"Pak SIDIK, S.Pd"},
            {jam:"09.40 – 10.00",ruang:"-",mapel:"🟡 ISTIRAHAT",guru:"-"},
            {jam:"10.00 – 10.30",ruang:"237",mapel:"Produktif TKJ",guru:"Pak FERRY LESMANA, S.Kom"},
            {jam:"10.30 – 11.10",ruang:"237",mapel:"Produktif TKJ",guru:"Pak FERRY LESMANA, S.Kom"},
            {jam:"11.10 – 11.50",ruang:"31",mapel:"Produktif TJKT",guru:"Pak M. NUGRAHA, S.Kom"},
            {jam:"11.50 – 12.30",ruang:"31",mapel:"Produktif TJKT",guru:"Pak M. NUGRAHA, S.Kom"}
        ],
        kamis: [
            {jam:"06.30 – 07.00",ruang:"-",mapel:"Kegiatan Pembiasaan",guru:"-"},
            {jam:"07.00 – 07.40",ruang:"57",mapel:"Bahasa Inggris",guru:"Bu OPAH NASOPAH, S.E"},
            {jam:"07.40 – 08.20",ruang:"57",mapel:"Bahasa Inggris",guru:"Bu OPAH NASOPAH, S.E"},
            {jam:"08.20 – 09.00",ruang:"57",mapel:"Bahasa Inggris",guru:"Bu OPAH NASOPAH, S.E"},
            {jam:"09.00 – 09.40",ruang:"29",mapel:"Bahasa Indonesia",guru:"Pak M. TONY, S.S"},
            {jam:"09.40 – 10.00",ruang:"-",mapel:"🟡 ISTIRAHAT",guru:"-"},
            {jam:"10.00 – 10.30",ruang:"29",mapel:"Bahasa Indonesia",guru:"Pak M. TONY, S.S"},
            {jam:"10.30 – 11.10",ruang:"29",mapel:"Bahasa Indonesia",guru:"Pak M. TONY, S.S"},
            {jam:"11.10 – 11.50",ruang:"239",mapel:"Sejarah",guru:"Bu SELVI SUKAESIH, S.Pd"},
            {jam:"11.50 – 12.30",ruang:"239",mapel:"Sejarah",guru:"Bu SELVI SUKAESIH, S.Pd"}
        ],
        jumat: [
            {jam:"06.30 – 07.00",ruang:"-",mapel:"Kegiatan Pembiasaan",guru:"-"},
            {jam:"07.00 – 07.40",ruang:"183",mapel:"BK / Bimbingan Konseling",guru:"Pak DONI BUDIYANTO BADRU TAMAM, S.Pd"},
            {jam:"07.40 – 08.20",ruang:"239",mapel:"Sejarah",guru:"Bu SELVI SUKAESIH, S.Pd"},
            {jam:"08.20 – 09.00",ruang:"16",mapel:"PKN",guru:"Pak SIDIK, S.Pd"},
            {jam:"09.00 – 09.40",ruang:"16",mapel:"PKN",guru:"Pak SIDIK, S.Pd"},
            {jam:"09.40 – 10.00",ruang:"-",mapel:"🟡 ISTIRAHAT",guru:"-"},
            {jam:"10.00 – 10.40",ruang:"31",mapel:"Produktif TJKT",guru:"Pak M. NUGRAHA, S.Kom"},
            {jam:"10.40 – 11.20",ruang:"31",mapel:"Produktif TJKT",guru:"Pak M. NUGRAHA, S.Kom"}
        ],
        sabtu: [
            {jam:"07.00 – 07.40",ruang:"237",mapel:"Produktif TKJ",guru:"Pak FERRY LESMANA, S.Kom"},
            {jam:"07.40 – 08.20",ruang:"237",mapel:"Produktif TKJ",guru:"Pak FERRY LESMANA, S.Kom"},
            {jam:"08.20 – 09.00",ruang:"183",mapel:"BK / Bimbingan Konseling",guru:"Pak DONI BUDIYANTO BADRU TAMAM, S.Pd"},
            {jam:"09.00 – 09.40",ruang:"183",mapel:"BK / Bimbingan Konseling",guru:"Pak DONI BUDIYANTO BADRU TAMAM, S.Pd"},
            {jam:"09.40 – 10.00",ruang:"-",mapel:"🟡 ISTIRAHAT",guru:"-"},
            {jam:"10.00 – 10.30",ruang:"151",mapel:"PJOK",guru:"Pak ZULIUS REMIT, S.Pd"},
            {jam:"10.30 – 11.10",ruang:"29",mapel:"Bahasa Indonesia",guru:"Pak M. TONY, S.S"},
            {jam:"11.10 – 11.50",ruang:"29",mapel:"Bahasa Indonesia",guru:"Pak M. TONY, S.S"}
        ]
    };

    function renderJadwal() {
        const hariMap = {senin:'senin',selasa:'selasa',rabu:'rabu',kamis:'kamis',jumat:'jumat',sabtu:'sabtu'};
        Object.keys(hariMap).forEach(key => {
            const tbody = document.getElementById(`jadwal${key.charAt(0).toUpperCase()+key.slice(1)}`);
            if (!tbody) return;
            const data = jadwalData[key];
            tbody.innerHTML = '';
            const header = document.createElement('tr');
            header.className = 'hari-label';
            header.innerHTML = `<td colspan="4"><i class="fas fa-calendar-day"></i> ${key.toUpperCase()}</td>`;
            tbody.appendChild(header);
            data.forEach(item => {
                const tr = document.createElement('tr');
                if (item.mapel.includes('ISTIRAHAT')) tr.className = 'istirahat';
                else if (item.mapel.includes('Kegiatan Pembiasaan') || item.mapel.includes('Upacara')) tr.className = 'pembiasaan';
                tr.innerHTML = `<td>${item.jam}</td><td>${item.ruang}</td><td>${item.mapel}</td><td class="guru">${item.guru}</td>`;
                tbody.appendChild(tr);
            });
        });
    }

    // ===== LOADING =====
    window.addEventListener('load', function() {
        setTimeout(() => document.getElementById('loadingOverlay').classList.add('hidden'), 1500);
        renderAnggota();
        renderJadwal();
        loadTugasSiswa();
        updateBadge();
        
        if (localStorage.getItem('isLoggedIn') === 'true') {
            const nama = localStorage.getItem('loggedInUser') || 'Siswa';
            document.getElementById('displayNamaSiswa').textContent = nama;
            document.getElementById('loginPage').classList.add('hidden');
            document.getElementById('mainContent').classList.add('show');
            document.getElementById('btnGuruNav').style.display = 'inline-block';
        }
        
        const savedGuru = localStorage.getItem('guruLogin');
        if (savedGuru && daftarGuru[savedGuru]) {
            guruLogin = savedGuru;
            isGuruMode = true;
            document.getElementById('loginPage').classList.add('hidden');
            document.getElementById('mainContent').classList.add('show');
            document.getElementById('btnGuruNav').style.display = 'inline-block';
            showGuruPanel(savedGuru);
        }
    });

    // ===== SWITCH TAB LOGIN =====
    function switchLoginTab(tab) {
        document.querySelectorAll('.login-tab').forEach(t => t.classList.remove('active'));
        document.querySelectorAll('.login-panel').forEach(p => p.classList.remove('active'));
        if (tab === 'siswa') {
            document.querySelector('.login-tab:first-child').classList.add('active');
            document.getElementById('panelSiswa').classList.add('active');
        } else {
            document.querySelector('.login-tab:last-child').classList.add('active');
            document.getElementById('panelGuru').classList.add('active');
        }
    }

    // ===== LOGIN SISWA =====
    function handleLoginSiswa(e) {
        e.preventDefault();
        const username = document.getElementById('username').value.trim();
        const password = document.getElementById('password').value.trim();
        const errorMsg = document.getElementById('errorMsgSiswa');
        const userValid = daftarSiswa.some(n => n.toLowerCase() === username.toLowerCase());
        if (userValid && password.toLowerCase() === 'xii tjkt 3') {
            errorMsg.classList.remove('show');
            localStorage.setItem('loggedInUser', username);
            localStorage.setItem('isLoggedIn', 'true');
            document.getElementById('displayNamaSiswa').textContent = username;
            document.getElementById('loginPage').classList.add('hidden');
            document.getElementById('mainContent').classList.add('show');
            document.getElementById('btnGuruNav').style.display = 'inline-block';
            if (isGuruMode) {
                isGuruMode = false;
                document.getElementById('guruPanel').style.display = 'none';
                document.getElementById('mainContent').style.display = 'block';
                document.getElementById('btnGuruNav').innerHTML = '<i class="fas fa-chalkboard-teacher"></i> Guru';
                document.getElementById('btnGuruNav').style.background = '#0b3b5c';
            }
        } else {
            errorMsg.classList.add('show');
        }
        return false;
    }

    // ===== LOGIN GURU =====
    function handleLoginGuru(e) {
        e.preventDefault();
        const nama = document.getElementById('guruLoginSelect').value;
        const password = document.getElementById('guruLoginPassword').value;
        const errorMsg = document.getElementById('errorMsgGuru');
        if (!nama) { errorMsg.textContent='Pilih guru!'; errorMsg.classList.add('show'); return false; }
        if (daftarGuru[nama] && daftarGuru[nama] === password) {
            errorMsg.classList.remove('show');
            guruLogin = nama;
            isGuruMode = true;
            localStorage.setItem('guruLogin', nama);
            document.getElementById('loginPage').classList.add('hidden');
            document.getElementById('mainContent').classList.add('show');
            document.getElementById('btnGuruNav').style.display = 'inline-block';
            showGuruPanel(nama);
        } else {
            errorMsg.textContent='Password salah!';
            errorMsg.classList.add('show');
        }
        return false;
    }

    // ===== TOGGLE GURU MODE =====
    function toggleGuruMode() {
        if (isGuruMode) {
            isGuruMode = false;
            document.getElementById('guruPanel').style.display = 'none';
            document.getElementById('mainContent').style.display = 'block';
            document.getElementById('btnGuruNav').innerHTML = '<i class="fas fa-chalkboard-teacher"></i> Guru';
            document.getElementById('btnGuruNav').style.background = '#0b3b5c';
            document.querySelectorAll('.content-section').forEach(s => s.style.display = 'block');
            document.getElementById('tugas').style.display = 'none';
            document.getElementById('tugasNav').classList.remove('active');
        } else {
            const nama = prompt('Masukkan nama guru:\n\nM. Nugraha\nMr. Tony\nRidwan Agus Tesyah\nMuhamad Khoerobi\nOpah Nasopah\nZulius Remit\nFerry Lesmana\nSidik\nSelvi Sukaesih\nDoni Budiyanto Badru Tamam');
            if (!nama) return;
            const pass = prompt('Masukkan password guru:');
            if (pass !== 'G7!mQ#2vL9@xR4pK') {
                alert('Password salah!');
                return;
            }
            guruLogin = nama;
            isGuruMode = true;
            localStorage.setItem('guruLogin', nama);
            document.getElementById('loginPage').classList.add('hidden');
            document.getElementById('mainContent').classList.add('show');
            document.getElementById('btnGuruNav').style.display = 'inline-block';
            showGuruPanel(nama);
        }
    }

    // ===== SHOW GURU PANEL =====
    function showGuruPanel(nama) {
        document.getElementById('mainContent').style.display = 'none';
        document.getElementById('guruPanel').style.display = 'block';
        document.getElementById('guruNameBadge').innerHTML = `<i class="fas fa-user-check"></i> ${nama}`;
        document.getElementById('btnGuruNav').innerHTML = '<i class="fas fa-sign-out-alt"></i> Keluar Guru';
        document.getElementById('btnGuruNav').style.background = '#c44545';
        loadGuruTugas(nama);
        updateGuruTugasSelect(nama);
        document.getElementById('guruPanel').scrollIntoView({behavior:'smooth',block:'start'});
    }

    // ===== LOGOUT =====
    function logout() {
        if (confirm('Yakin ingin logout?')) {
            localStorage.removeItem('isLoggedIn');
            localStorage.removeItem('loggedInUser');
            if (isGuruMode) {
                localStorage.removeItem('guruLogin');
                isGuruMode = false;
                document.getElementById('guruPanel').style.display = 'none';
                document.getElementById('mainContent').style.display = 'block';
                document.getElementById('btnGuruNav').innerHTML = '<i class="fas fa-chalkboard-teacher"></i> Guru';
                document.getElementById('btnGuruNav').style.background = '#0b3b5c';
            }
            document.getElementById('mainContent').classList.remove('show');
            document.getElementById('loginPage').classList.remove('hidden');
        }
    }

    // ===== LOGOUT GURU =====
    function logoutGuru() {
        if (confirm('Yakin ingin logout dari dashboard guru?')) {
            localStorage.removeItem('guruLogin');
            isGuruMode = false;
            document.getElementById('guruPanel').style.display = 'none';
            document.getElementById('mainContent').style.display = 'block';
            document.getElementById('btnGuruNav').innerHTML = '<i class="fas fa-chalkboard-teacher"></i> Guru';
            document.getElementById('btnGuruNav').style.background = '#0b3b5c';
            document.querySelectorAll('.content-section').forEach(s => s.style.display = 'block');
            document.getElementById('tugas').style.display = 'none';
            document.getElementById('tugasNav').classList.remove('active');
            if (localStorage.getItem('isLoggedIn') !== 'true') {
                document.getElementById('mainContent').classList.remove('show');
                document.getElementById('loginPage').classList.remove('hidden');
            }
        }
    }

    // ===== NAVIGASI =====
    const sections = document.querySelectorAll('.content-section');
    const navLinks = document.querySelectorAll('.nav-links a');

    function updateActiveLink() {
        let current = '';
        sections.forEach(s => {
            const top = s.offsetTop - 150;
            const bottom = top + s.offsetHeight;
            if (window.scrollY >= top && window.scrollY < bottom) current = s.id;
        });
        navLinks.forEach(l => {
            l.classList.remove('active');
            if (l.getAttribute('href') === `#${current}`) l.classList.add('active');
        });
    }

    window.addEventListener('scroll', updateActiveLink);

    const scrollBtn = document.getElementById('scrollTopBtn');
    window.addEventListener('scroll', () => scrollBtn.classList.toggle('show', window.scrollY > 300));
    function scrollToTop() { window.scrollTo({top:0,behavior:'smooth'}); }

    navLinks.forEach(l => {
        l.addEventListener('click', function(e) {
            e.preventDefault();
            const targetId = this.getAttribute('href');
            const target = document.querySelector(targetId);
            if (target) {
                if (targetId === '#tugas') {
                    document.querySelectorAll('.content-section').forEach(s => {
                        if (s.id !== 'tugas') s.style.display = 'none';
                    });
                    document.getElementById('tugas').style.display = 'block';
                    document.getElementById('tugas').classList.add('active');
                    loadTugasSiswa();
                    navLinks.forEach(link => link.classList.remove('active'));
                    this.classList.add('active');
                    window.scrollTo({top: 0, behavior: 'smooth'});
                } else {
                    document.querySelectorAll('.content-section').forEach(s => {
                        if (s.id !== 'tugas') s.style.display = 'block';
                    });
                    document.getElementById('tugas').style.display = 'none';
                    document.getElementById('tugas').classList.remove('active');
                    target.scrollIntoView({behavior:'smooth',block:'start'});
                    navLinks.forEach(link => link.classList.remove('active'));
                    this.classList.add('active');
                }
            }
        });
    });

    // ===== MODE TOGGLE =====
    function toggleMode() {
        const body = document.body;
        const btn = document.getElementById('modeToggle');
        body.classList.toggle('mode-hp');
        if (body.classList.contains('mode-hp')) {
            btn.innerHTML = '<i class="fas fa-exchange-alt"></i> Laptop';
            localStorage.setItem('mode', 'hp');
        } else {
            btn.innerHTML = '<i class="fas fa-exchange-alt"></i> HP';
            localStorage.setItem('mode', 'laptop');
        }
    }
    if (localStorage.getItem('mode') === 'hp') {
        document.body.classList.add('mode-hp');
        document.getElementById('modeToggle').innerHTML = '<i class="fas fa-exchange-alt"></i> Laptop';
    }

    // ===== FUNGSI GURU =====
    function buatTugas() { buatTugasUjian('Tugas'); }
    function buatUjian() { buatTugasUjian('Ujian'); }

    function buatTugasUjian(jenis) {
        if (!guruLogin) { alert('Login guru dulu!'); return; }
        
        const judul = prompt('Masukkan judul ' + jenis + ':');
        if (!judul) return;
        
        const deadline = prompt('Masukkan deadline (contoh: 2026-08-22) atau kosongkan:') || 'Tidak ada batas';
        
        let pertanyaan = [];
        let no = 1;
        while (true) {
            const tanya = prompt('Masukkan pertanyaan ke-' + no + ' (kosongkan untuk selesai):');
            if (!tanya) break;
            pertanyaan.push({ no: no, text: tanya });
            no++;
        }
        
        if (pertanyaan.length === 0) {
            alert('Minimal 1 pertanyaan!');
            return;
        }
        
        let tugasData = JSON.parse(localStorage.getItem('tugasData')) || {};
        if (!tugasData[guruLogin]) tugasData[guruLogin] = [];
        tugasData[guruLogin].push({
            id: Date.now(),
            guru: guruLogin,
            jenis: jenis,
            judul: judul,
            pertanyaan: pertanyaan,
            deadline: deadline,
            tanggal: new Date().toLocaleDateString('id-ID'),
            waktu: new Date().toLocaleTimeString('id-ID', {hour:'2-digit',minute:'2-digit'})
        });
        localStorage.setItem('tugasData', JSON.stringify(tugasData));
        loadGuruTugas(guruLogin);
        updateGuruTugasSelect(guruLogin);
        loadTugasSiswa();
        updateBadge();
        alert('✅ ' + jenis + ' dengan ' + pertanyaan.length + ' pertanyaan berhasil ditambahkan!');
    }

    function loadGuruTugas(nama) {
        const container = document.getElementById('guruTugasList');
        let tugasData = JSON.parse(localStorage.getItem('tugasData')) || {};
        const data = tugasData[nama] || [];
        if (data.length === 0) {
            container.innerHTML = '<p style="color:#7a94b0;text-align:center;padding:1rem;">Belum ada tugas/ujian.</p>';
            return;
        }
        container.innerHTML = '';
        data.slice().reverse().forEach(item => {
            const div = document.createElement('div');
            div.className = 'tugas-item-guru';
            const jml = item.pertanyaan ? item.pertanyaan.length : 0;
            div.innerHTML = `
                <div class="info">
                    <strong>${item.judul}</strong>
                    <span class="badge ${item.jenis === 'Ujian' ? 'ujian' : 'tugas'}">${item.jenis}</span>
                    <span style="font-size:0.7rem;color:#4b6a89;">(${jml} pertanyaan)</span>
                    <div class="meta">Deadline: ${item.deadline} • ${item.tanggal}</div>
                </div>
                <div class="actions">
                    <button onclick="hapusTugasGuru(${item.id})"><i class="fas fa-trash"></i></button>
                </div>
            `;
            container.appendChild(div);
        });
    }

    function hapusTugasGuru(id) {
        if (!confirm('Hapus tugas ini?')) return;
        let tugasData = JSON.parse(localStorage.getItem('tugasData')) || {};
        tugasData[guruLogin] = tugasData[guruLogin].filter(i => i.id !== id);
        localStorage.setItem('tugasData', JSON.stringify(tugasData));
        loadGuruTugas(guruLogin);
        updateGuruTugasSelect(guruLogin);
        loadTugasSiswa();
        updateBadge();
    }

    function updateGuruTugasSelect(nama) {
        const select = document.getElementById('guruPilihTugas');
        if (!select) return;
        select.innerHTML = '<option value="">-- Pilih tugas --</option>';
        let tugasData = JSON.parse(localStorage.getItem('tugasData')) || {};
        const data = tugasData[nama] || [];
        data.forEach(item => {
            const opt = document.createElement('option');
            opt.value = item.id;
            opt.textContent = `${item.judul} (${item.jenis})`;
            select.appendChild(opt);
        });
    }

    // ===== LOAD SISWA NILAI (GURU) =====
    function loadSiswaNilaiGuru() {
        const container = document.getElementById('guruSiswaNilai');
        const tugasId = document.getElementById('guruPilihTugas').value;
        if (!tugasId) { container.innerHTML = '<p style="color:#7a94b0;text-align:center;padding:1rem;">Pilih tugas untuk melihat siswa</p>'; return; }
        
        let nilaiData = JSON.parse(localStorage.getItem('nilaiData')) || {};
        let jawabanData = JSON.parse(localStorage.getItem('jawabanSiswa')) || {};
        
        let tugasItem = null;
        let tugasData = JSON.parse(localStorage.getItem('tugasData')) || {};
        Object.keys(tugasData).forEach(g => {
            const found = tugasData[g].find(i => i.id == tugasId);
            if (found) tugasItem = found;
        });
        
        let html = `<p style="color:#4b6a89;font-size:0.9rem;margin-bottom:0.5rem;">📌 <strong>${tugasItem ? tugasItem.judul : 'Tugas'}</strong> - ${tugasItem ? tugasItem.jenis : ''}</p>`;
        
        const hasPertanyaan = tugasItem && tugasItem.pertanyaan && tugasItem.pertanyaan.length > 0;
        
        if (!hasPertanyaan) {
            html += `<div style="background:#fff3cd;border-radius:8px;padding:0.8rem;margin-bottom:1rem;color:#856404;">
                <i class="fas fa-info-circle"></i> Tugas ini tidak memiliki pertanyaan. 
                <strong>Buat tugas baru</strong> dengan tombol "Tugas" atau "Ujian" untuk bisa dijawab siswa.
            </div>`;
        }
        
        let sudahMengerjakan = 0;
        let totalSiswa = daftarSiswa.length;
        
        daftarSiswa.forEach(nama => {
            const key = `${tugasId}_${nama}`;
            const nilai = nilaiData[key] || '';
            const jawaban = jawabanData[key];
            const status = jawaban ? 'sudah' : 'belum';
            const statusText = jawaban ? '✅ Sudah' : '⏳ Belum';
            if (jawaban) sudahMengerjakan++;
            
            let jawabanHTML = '';
            if (jawaban && jawaban.jawaban) {
                jawabanHTML = `
                    <div class="jawaban-box">
                        ${Object.keys(jawaban.jawaban).map(no => `
                            <div class="jawaban-item">
                                <span class="q">${no}.</span> <span class="a">${jawaban.jawaban[no]}</span>
                            </div>
                        `).join('')}
                        <div class="waktu">⏰ Dikirim: ${jawaban.tanggal} ${jawaban.waktu}</div>
                    </div>
                `;
            } else if (hasPertanyaan) {
                jawabanHTML = `<div class="tidak-ada-jawaban">Belum mengerjakan</div>`;
            }
            
            html += `
                <div class="siswa-item-guru">
                    <div class="row">
                        <span style="font-weight:600;font-size:0.9rem;">${nama}</span>
                        <span class="status ${status}">${statusText}</span>
                        <div style="display:flex;align-items:center;gap:0.3rem;">
                            <span style="font-weight:700;color:#0b3b5c;min-width:25px;">${nilai || '-'}</span>
                            <input type="number" min="0" max="100" placeholder="Nilai" value="${nilai}" 
                                   onchange="simpanNilaiGuru('${tugasId}','${nama}',this.value)" />
                        </div>
                    </div>
                    ${hasPertanyaan ? jawabanHTML : ''}
                </div>
            `;
        });
        
        if (hasPertanyaan) {
            html = `<div style="background:#e8f5e9;border-radius:8px;padding:0.5rem 1rem;margin-bottom:1rem;">
                <span style="font-weight:600;">📊 Statistik:</span> 
                <span style="color:#2e7d32;">${sudahMengerjakan}</span> siswa sudah mengerjakan dari 
                <span style="color:#0b3b5c;">${totalSiswa}</span> siswa
                <span style="margin-left:1rem;font-size:0.8rem;color:#4b6a89;">(${Math.round(sudahMengerjakan/totalSiswa*100)}%)</span>
            </div>` + html;
        }
        
        container.innerHTML = html;
    }

    function simpanNilaiGuru(tugasId, nama, nilai) {
        let nilaiData = JSON.parse(localStorage.getItem('nilaiData')) || {};
        const key = `${tugasId}_${nama}`;
        if (nilai && nilai >= 0 && nilai <= 100) {
            nilaiData[key] = nilai;
        } else if (nilai === '') {
            delete nilaiData[key];
        } else {
            alert('Nilai harus antara 0-100');
            return;
        }
        localStorage.setItem('nilaiData', JSON.stringify(nilaiData));
        loadSiswaNilaiGuru();
    }

    // ===== TUGAS SISWA =====
    function loadTugasSiswa() {
        const container = document.getElementById('tugasSiswaContainer');
        const tugasData = JSON.parse(localStorage.getItem('tugasData')) || {};
        const allTugas = [];
        Object.keys(tugasData).forEach(g => { if (tugasData[g]) tugasData[g].forEach(i => allTugas.push(i)); });
        allTugas.sort((a,b) => b.id - a.id);
        if (allTugas.length === 0) {
            container.innerHTML = `<p style="color:#7a94b0;text-align:center;padding:2rem;"><i class="fas fa-inbox" style="font-size:2rem;display:block;margin-bottom:0.5rem;"></i>Belum ada tugas atau ujian.</p>`;
            updateBadge(0); return;
        }
        container.innerHTML = '';
        const siswa = localStorage.getItem('loggedInUser') || 'Siswa';
        
        allTugas.forEach(item => {
            const card = document.createElement('div');
            card.className = 'tugas-siswa-card';
            
            let jawabanData = JSON.parse(localStorage.getItem('jawabanSiswa')) || {};
            const key = `${item.id}_${siswa}`;
            const sudahJawab = jawabanData[key] ? true : false;
            
            const hasPertanyaan = item.pertanyaan && item.pertanyaan.length > 0;
            
            let jawabanHTML = '';
            if (hasPertanyaan) {
                jawabanHTML = `<div style="margin-top:0.8rem;border-top:1px solid #e9eef3;padding-top:0.8rem;">
                    <h4 style="font-size:0.9rem;color:#0b3b5c;margin-bottom:0.5rem;">📝 Jawab Pertanyaan:</h4>`;
                item.pertanyaan.forEach(p => {
                    const savedAnswer = jawabanData[key] ? jawabanData[key].jawaban[p.no] || '' : '';
                    jawabanHTML += `
                        <div style="margin-bottom:0.5rem;">
                            <p style="font-weight:600;font-size:0.85rem;">${p.no}. ${p.text}</p>
                            <textarea id="jawaban_${item.id}_${p.no}" rows="2" 
                                ${sudahJawab ? 'disabled style="background:#f4faff;"' : ''}
                            >${savedAnswer}</textarea>
                        </div>
                    `;
                });
                jawabanHTML += `
                    ${sudahJawab ? 
                        `<p class="status-sudah"><i class="fas fa-check-circle"></i> Jawaban sudah dikirim!</p>` :
                        `<button onclick="kirimJawaban(${item.id})" class="btn-kirim"><i class="fas fa-paper-plane"></i> Kirim Jawaban</button>
                         <div id="statusKirim_${item.id}" style="margin-top:0.3rem;font-size:0.8rem;color:#2e7d32;display:none;">✅ Jawaban terkirim!</div>`
                    }
                `;
            } else {
                jawabanHTML = `
                    <div class="info-tugas-lama">
                        <i class="fas fa-info-circle"></i> Tugas ini hanya berupa pengumuman. 
                        <strong>Guru perlu membuat tugas baru</strong> dengan fitur "Tugas" atau "Ujian" untuk bisa dijawab.
                    </div>
                `;
            }
            
            card.innerHTML = `
                <div class="guru-name">👨‍🏫 ${item.guru} • ${item.tanggal}</div>
                <div class="judul">${item.judul}</div>
                <span class="badge ${item.jenis === 'Ujian' ? 'ujian' : 'tugas'}">${item.jenis}</span>
                <div class="meta"><span>⏰ Deadline: ${item.deadline}</span><span>🕐 ${item.waktu}</span></div>
                ${jawabanHTML}
            `;
            container.appendChild(card);
        });
        updateBadge(allTugas.length);
    }

    function kirimJawaban(tugasId) {
        const siswa = localStorage.getItem('loggedInUser') || 'Siswa';
        let jawabanData = JSON.parse(localStorage.getItem('jawabanSiswa')) || {};
        const key = `${tugasId}_${siswa}`;
        
        let jawaban = {};
        const tugasData = JSON.parse(localStorage.getItem('tugasData')) || {};
        let tugasItem = null;
        Object.keys(tugasData).forEach(g => {
            const found = tugasData[g].find(i => i.id === tugasId);
            if (found) tugasItem = found;
        });
        
        if (tugasItem && tugasItem.pertanyaan) {
            tugasItem.pertanyaan.forEach(p => {
                const el = document.getElementById(`jawaban_${tugasId}_${p.no}`);
                if (el) {
                    jawaban[p.no] = el.value.trim() || '(kosong)';
                }
            });
        }
        
        jawabanData[key] = {
            siswa: siswa,
            tugasId: tugasId,
            jawaban: jawaban,
            tanggal: new Date().toLocaleDateString('id-ID'),
            waktu: new Date().toLocaleTimeString('id-ID', {hour:'2-digit',minute:'2-digit'})
        };
        
        localStorage.setItem('jawabanSiswa', JSON.stringify(jawabanData));
        
        const statusEl = document.getElementById(`statusKirim_${tugasId}`);
        if (statusEl) {
            statusEl.style.display = 'block';
            setTimeout(() => { statusEl.style.display = 'none'; }, 3000);
        }
        alert('✅ Jawaban berhasil dikirim!');
        loadTugasSiswa();
    }

    function updateBadge(count) {
        const badge = document.getElementById('tugasBadge');
        if (!badge) return;
        if (count === undefined) {
            const tugasData = JSON.parse(localStorage.getItem('tugasData')) || {};
            let total = 0;
            Object.keys(tugasData).forEach(g => { if (tugasData[g]) total += tugasData[g].length; });
            count = total;
        }
        badge.textContent = count;
    }

    // ===== CEK SESSION =====
    if (localStorage.getItem('isLoggedIn') === 'true') {
        const nama = localStorage.getItem('loggedInUser') || 'Siswa';
        document.getElementById('displayNamaSiswa').textContent = nama;
        document.getElementById('loginPage').classList.add('hidden');
        document.getElementById('mainContent').classList.add('show');
        document.getElementById('btnGuruNav').style.display = 'inline-block';
    }
    const savedGuru = localStorage.getItem('guruLogin');
    if (savedGuru && daftarGuru[savedGuru]) {
        guruLogin = savedGuru;
        isGuruMode = true;
        document.getElementById('loginPage').classList.add('hidden');
        document.getElementById('mainContent').classList.add('show');
        document.getElementById('btnGuruNav').style.display = 'inline-block';
        showGuruPanel(savedGuru);
    }

    console.log('📚 Website XII TJKT 3 siap!');
    console.log('🔑 Password siswa: xii tjkt 3');
    console.log('🔑 Password guru: G7!mQ#2vL9@xR4pK');
    console.log('🔥 Firebase Connected! Data siap disimpan di cloud.');
</script>

</body>
</html>
