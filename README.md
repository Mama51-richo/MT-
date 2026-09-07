<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
    <title>MYBIRR - Digital Wallet</title>
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">
    <link href="https://fonts.googleapis.com/css2?family=Inter:wght@300;400;500;600;700;800;900&display=swap" rel="stylesheet">
    <style>
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
        }

        :root {
            --primary: #6C3CE1;
            --primary-dark: #5A2FC9;
            --primary-light: #8B6CE8;
            --primary-gradient: linear-gradient(135deg, #6C3CE1 0%, #4A1DB8 100%);
            --secondary: #00C9A7;
            --background: #F8F9FE;
            --surface: #FFFFFF;
            --text-primary: #1A1A2E;
            --text-secondary: #6B7280;
            --text-light: #9CA3AF;
            --border: #E5E7EB;
            --shadow: 0 4px 20px rgba(108, 60, 225, 0.15);
            --radius: 20px;
            --radius-sm: 12px;
            --transition: all 0.3s cubic-bezier(0.4, 0, 0.2, 1);
            --safe-bottom: env(safe-area-inset-bottom, 0px);
        }

        body {
            font-family: 'Inter', -apple-system, BlinkMacSystemFont, sans-serif;
            background: var(--background);
            color: var(--text-primary);
            margin: 0;
            padding: 0;
            overflow: hidden;
            height: 100vh;
            width: 100vw;
            position: relative;
        }

        #app {
            position: relative;
            width: 100%;
            height: 100vh;
            max-width: 430px;
            margin: 0 auto;
            overflow: hidden;
            background: var(--background);
        }

        .screen {
            position: absolute;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            overflow-y: auto;
            overflow-x: hidden;
            background: var(--background);
            opacity: 0;
            visibility: hidden;
            transform: translateX(30px);
            transition: all 0.4s cubic-bezier(0.4, 0, 0.2, 1);
            padding-bottom: 80px;
            -webkit-overflow-scrolling: touch;
        }

        .screen.active {
            opacity: 1;
            visibility: visible;
            transform: translateX(0);
            z-index: 10;
        }

        .screen::-webkit-scrollbar {
            width: 3px;
        }
        .screen::-webkit-scrollbar-thumb {
            background: var(--primary-light);
            border-radius: 10px;
        }

        /* Splash */
        #splash-screen {
            background: var(--primary-gradient);
            display: flex;
            align-items: center;
            justify-content: center;
            z-index: 1000;
            padding: 0;
        }

        .splash-content {
            text-align: center;
            animation: fadeInUp 0.8s ease;
        }

        .splash-logo {
            display: flex;
            align-items: center;
            justify-content: center;
            gap: 12px;
            margin-bottom: 30px;
        }

        .splash-logo i {
            font-size: 48px;
            color: white;
            background: rgba(255, 255, 255, 0.2);
            padding: 15px;
            border-radius: 16px;
            backdrop-filter: blur(10px);
        }

        .splash-logo span {
            font-size: 42px;
            font-weight: 900;
            color: white;
            letter-spacing: -1px;
        }

        .splash-text {
            color: rgba(255, 255, 255, 0.8);
            font-size: 16px;
            margin-top: 10px;
        }

        .loader {
            width: 40px;
            height: 40px;
            border: 3px solid rgba(255, 255, 255, 0.2);
            border-top: 3px solid white;
            border-radius: 50%;
            animation: spin 1s linear infinite;
            margin: 30px auto;
        }

        @keyframes spin {
            0% { transform: rotate(0deg); }
            100% { transform: rotate(360deg); }
        }

        @keyframes fadeInUp {
            from { opacity: 0; transform: translateY(30px); }
            to { opacity: 1; transform: translateY(0); }
        }

        /* Onboarding */
        .onboarding-container {
            height: 100%;
            display: flex;
            flex-direction: column;
            justify-content: center;
            padding: 40px 30px;
        }

        .onboarding-slides {
            flex: 1;
            display: flex;
            overflow: hidden;
            position: relative;
        }

        .slide {
            position: absolute;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            display: flex;
            flex-direction: column;
            align-items: center;
            justify-content: center;
            opacity: 0;
            transform: translateX(50px);
            transition: all 0.6s cubic-bezier(0.4, 0, 0.2, 1);
            padding: 20px;
        }

        .slide.active {
            opacity: 1;
            transform: translateX(0);
        }

        .slide-icon {
            width: 120px;
            height: 120px;
            background: var(--primary-gradient);
            border-radius: 50%;
            display: flex;
            align-items: center;
            justify-content: center;
            margin-bottom: 40px;
            box-shadow: 0 20px 60px rgba(108, 60, 225, 0.3);
        }

        .slide-icon i {
            font-size: 50px;
            color: white;
        }

        .slide h2 {
            font-size: 28px;
            font-weight: 800;
            margin-bottom: 16px;
            text-align: center;
            color: var(--text-primary);
        }

        .slide p {
            font-size: 16px;
            color: var(--text-secondary);
            text-align: center;
            line-height: 1.6;
            max-width: 320px;
        }

        .onboarding-dots {
            display: flex;
            justify-content: center;
            gap: 10px;
            margin: 30px 0;
        }

        .dot {
            width: 8px;
            height: 8px;
            border-radius: 50%;
            background: var(--border);
            transition: all 0.3s ease;
            cursor: pointer;
        }

        .dot.active {
            background: var(--primary);
            width: 30px;
            border-radius: 4px;
        }

        .onboarding-buttons {
            display: flex;
            gap: 15px;
            padding: 0 10px;
        }

        .btn-skip {
            flex: 1;
            padding: 16px;
            background: var(--surface);
            border: 2px solid var(--border);
            border-radius: var(--radius-sm);
            font-size: 16px;
            font-weight: 600;
            color: var(--text-secondary);
            cursor: pointer;
            transition: var(--transition);
        }
        .btn-skip:hover {
            border-color: var(--primary);
            color: var(--primary);
        }

        .btn-next {
            flex: 2;
            padding: 16px;
            background: var(--primary-gradient);
            border: none;
            border-radius: var(--radius-sm);
            font-size: 16px;
            font-weight: 600;
            color: white;
            cursor: pointer;
            transition: var(--transition);
            box-shadow: 0 4px 15px rgba(108, 60, 225, 0.3);
        }
        .btn-next:hover {
            transform: translateY(-2px);
            box-shadow: 0 8px 25px rgba(108, 60, 225, 0.4);
        }

        /* Auth */
        .auth-container {
            padding: 40px 25px;
            height: 100%;
            display: flex;
            flex-direction: column;
            justify-content: center;
        }

        .auth-header {
            text-align: center;
            margin-bottom: 40px;
        }

        .auth-logo {
            display: inline-flex;
            align-items: center;
            gap: 10px;
            margin-bottom: 20px;
        }
        .auth-logo i {
            font-size: 32px;
            color: var(--primary);
        }
        .auth-logo span {
            font-size: 28px;
            font-weight: 800;
            color: var(--text-primary);
        }

        .auth-header h1 {
            font-size: 28px;
            font-weight: 800;
            margin-bottom: 8px;
        }
        .auth-header p {
            color: var(--text-secondary);
            font-size: 15px;
        }

        /* Forms */
        .form-group {
            margin-bottom: 20px;
        }

        .form-group label {
            display: block;
            font-size: 14px;
            font-weight: 600;
            margin-bottom: 8px;
            color: var(--text-primary);
        }

        .form-group input,
        .form-group select {
            width: 100%;
            padding: 14px 16px;
            border: 2px solid var(--border);
            border-radius: var(--radius-sm);
            font-size: 16px;
            font-family: inherit;
            transition: var(--transition);
            background: var(--surface);
            color: var(--text-primary);
        }

        .form-group input:focus,
        .form-group select:focus {
            outline: none;
            border-color: var(--primary);
            box-shadow: 0 0 0 4px rgba(108, 60, 225, 0.1);
        }

        .input-group {
            position: relative;
            display: flex;
            align-items: center;
        }

        .input-prefix {
            position: absolute;
            left: 16px;
            font-weight: 600;
            color: var(--text-secondary);
        }

        .input-group input {
            padding-left: 70px;
        }

        .input-icon {
            position: absolute;
            right: 16px;
            cursor: pointer;
            color: var(--text-light);
            transition: var(--transition);
        }
        .input-icon:hover {
            color: var(--primary);
        }

        .form-options {
            display: flex;
            justify-content: space-between;
            align-items: center;
            margin: 20px 0;
        }

        .checkbox-label {
            display: flex;
            align-items: center;
            gap: 8px;
            font-size: 14px;
            color: var(--text-secondary);
            cursor: pointer;
        }
        .checkbox-label input[type="checkbox"] {
            width: 18px;
            height: 18px;
            accent-color: var(--primary);
            cursor: pointer;
        }

        .btn-primary {
            padding: 16px;
            background: var(--primary-gradient);
            border: none;
            border-radius: var(--radius-sm);
            font-size: 16px;
            font-weight: 600;
            color: white;
            cursor: pointer;
            transition: var(--transition);
            box-shadow: 0 4px 15px rgba(108, 60, 225, 0.3);
        }
        .btn-primary:hover {
            transform: translateY(-2px);
            box-shadow: 0 8px 25px rgba(108, 60, 225, 0.4);
        }
        .btn-primary:active {
            transform: translateY(0);
        }

        .btn-full {
            width: 100%;
        }

        .btn-secondary {
            padding: 14px 24px;
            background: var(--surface);
            border: 2px solid var(--border);
            border-radius: var(--radius-sm);
            font-size: 15px;
            font-weight: 600;
            color: var(--text-secondary);
            cursor: pointer;
            transition: var(--transition);
            display: inline-flex;
            align-items: center;
            gap: 8px;
        }
        .btn-secondary:hover {
            border-color: var(--primary);
            color: var(--primary);
        }

        .auth-footer {
            text-align: center;
            margin-top: 30px;
            font-size: 15px;
            color: var(--text-secondary);
        }
        .auth-footer a {
            color: var(--primary);
            text-decoration: none;
            font-weight: 600;
        }
        .auth-footer a:hover {
            text-decoration: underline;
        }

        /* OTP */
        .otp-inputs {
            display: flex;
            gap: 12px;
            justify-content: center;
            margin: 30px 0;
        }

        .otp-input {
            width: 50px;
            height: 60px;
            text-align: center;
            font-size: 24px;
            font-weight: 700;
            border: 2px solid var(--border);
            border-radius: var(--radius-sm);
            transition: var(--transition);
            background: var(--surface);
            color: var(--text-primary);
        }
        .otp-input:focus {
            outline: none;
            border-color: var(--primary);
            box-shadow: 0 0 0 4px rgba(108, 60, 225, 0.1);
            transform: translateY(-2px);
        }

        /* Home */
        .app-header {
            display: flex;
            justify-content: space-between;
            align-items: center;
            padding: 20px 20px 10px;
            position: sticky;
            top: 0;
            background: var(--background);
            z-index: 50;
        }

        .app-logo {
            display: flex;
            align-items: center;
            gap: 8px;
        }
        .app-logo i {
            font-size: 28px;
            color: var(--primary);
        }
        .app-logo span {
            font-size: 22px;
            font-weight: 800;
            color: var(--text-primary);
        }

        .header-right {
            display: flex;
            align-items: center;
            gap: 15px;
        }

        .icon-btn {
            width: 40px;
            height: 40px;
            border: none;
            background: var(--surface);
            border-radius: 50%;
            display: flex;
            align-items: center;
            justify-content: center;
            cursor: pointer;
            transition: var(--transition);
            position: relative;
            box-shadow: 0 2px 10px rgba(0, 0, 0, 0.05);
        }
        .icon-btn:hover {
            transform: scale(1.05);
            background: var(--primary-light);
            color: white;
        }

        .icon-btn .badge {
            position: absolute;
            top: -2px;
            right: -2px;
            background: #FF3B30;
            color: white;
            font-size: 10px;
            font-weight: 700;
            width: 18px;
            height: 18px;
            border-radius: 50%;
            display: flex;
            align-items: center;
            justify-content: center;
            border: 2px solid var(--background);
        }

        .user-avatar {
            width: 40px;
            height: 40px;
            border-radius: 50%;
            background: var(--primary-gradient);
            display: flex;
            align-items: center;
            justify-content: center;
            color: white;
            font-weight: 600;
            cursor: pointer;
            transition: var(--transition);
        }
        .user-avatar:hover {
            transform: scale(1.05);
        }

        /* Balance Card */
        .balance-card {
            background: var(--primary-gradient);
            margin: 10px 20px;
            padding: 25px;
            border-radius: var(--radius);
            color: white;
            box-shadow: 0 10px 40px rgba(108, 60, 225, 0.3);
            position: relative;
            overflow: hidden;
        }

        .balance-card::before {
            content: '';
            position: absolute;
            top: -50%;
            right: -30%;
            width: 200px;
            height: 200px;
            background: rgba(255, 255, 255, 0.05);
            border-radius: 50%;
        }

        .balance-card::after {
            content: '';
            position: absolute;
            bottom: -40%;
            left: -20%;
            width: 150px;
            height: 150px;
            background: rgba(255, 255, 255, 0.05);
            border-radius: 50%;
        }

        .balance-header {
            display: flex;
            justify-content: space-between;
            align-items: center;
            margin-bottom: 12px;
        }

        .balance-label {
            font-size: 14px;
            opacity: 0.8;
            font-weight: 500;
        }

        .balance-card .icon-btn {
            background: rgba(255, 255, 255, 0.15);
            color: white;
            width: 36px;
            height: 36px;
            box-shadow: none;
        }
        .balance-card .icon-btn:hover {
            background: rgba(255, 255, 255, 0.25);
        }

        .balance-amount {
            font-size: 36px;
            font-weight: 800;
            margin: 10px 0 20px;
            letter-spacing: -1px;
        }

        .balance-amount .currency {
            font-size: 20px;
            font-weight: 600;
            opacity: 0.8;
            margin-right: 5px;
        }

        .balance-details {
            display: flex;
            gap: 30px;
            padding-top: 15px;
            border-top: 1px solid rgba(255, 255, 255, 0.15);
        }

        .balance-item {
            display: flex;
            flex-direction: column;
        }

        .balance-item .label {
            font-size: 11px;
            opacity: 0.7;
            text-transform: uppercase;
            letter-spacing: 0.5px;
        }

        .balance-item .value {
            font-size: 14px;
            font-weight: 600;
            margin-top: 3px;
        }

        .badge-verified {
            background: rgba(0, 200, 167, 0.3);
            padding: 2px 12px;
            border-radius: 20px;
            font-size: 11px;
        }

        /* Quick Actions */
        .quick-actions {
            display: grid;
            grid-template-columns: repeat(4, 1fr);
            gap: 12px;
            padding: 15px 20px;
        }

        .action-btn {
            display: flex;
            flex-direction: column;
            align-items: center;
            gap: 8px;
            background: none;
            border: none;
            cursor: pointer;
            transition: var(--transition);
        }
        .action-btn:hover {
            transform: translateY(-3px);
        }

        .action-icon {
            width: 56px;
            height: 56px;
            border-radius: 16px;
            display: flex;
            align-items: center;
            justify-content: center;
            font-size: 22px;
            transition: var(--transition);
        }

        .action-icon.send {
            background: rgba(108, 60, 225, 0.1);
            color: var(--primary);
        }
        .action-icon.receive {
            background: rgba(0, 201, 167, 0.1);
            color: var(--secondary);
        }
        .action-icon.scan {
            background: rgba(255, 152, 0, 0.1);
            color: #FF9800;
        }
        .action-icon.pay {
            background: rgba(233, 30, 99, 0.1);
            color: #E91E63;
        }

        .action-btn span {
            font-size: 12px;
            font-weight: 600;
            color: var(--text-secondary);
        }

        /* Services */
        .services-grid {
            display: grid;
            grid-template-columns: repeat(4, 1fr);
            gap: 10px;
            padding: 10px 20px 20px;
        }

        .service-btn {
            display: flex;
            flex-direction: column;
            align-items: center;
            gap: 6px;
            background: var(--surface);
            border: none;
            border-radius: var(--radius-sm);
            padding: 12px 8px;
            cursor: pointer;
            transition: var(--transition);
            box-shadow: 0 2px 10px rgba(0, 0, 0, 0.04);
        }
        .service-btn:hover {
            transform: translateY(-2px);
            box-shadow: var(--shadow);
        }

        .service-btn i {
            font-size: 20px;
            color: var(--primary);
        }

        .service-btn span {
            font-size: 11px;
            font-weight: 500;
            color: var(--text-secondary);
        }

        /* Recent Transactions */
        .recent-transactions {
            padding: 0 20px 20px;
        }

        .section-header {
            display: flex;
            justify-content: space-between;
            align-items: center;
            margin-bottom: 15px;
        }

        .section-header h3 {
            font-size: 18px;
            font-weight: 700;
        }

        .section-header a {
            color: var(--primary);
            text-decoration: none;
            font-size: 14px;
            font-weight: 600;
        }

        .transaction-list {
            display: flex;
            flex-direction: column;
            gap: 8px;
        }

        .transaction-item {
            display: flex;
            align-items: center;
            gap: 14px;
            background: var(--surface);
            padding: 14px 16px;
            border-radius: var(--radius-sm);
            transition: var(--transition);
            cursor: pointer;
        }
        .transaction-item:hover {
            background: var(--border);
        }

        .tx-icon {
            width: 44px;
            height: 44px;
            border-radius: 50%;
            display: flex;
            align-items: center;
            justify-content: center;
            font-size: 18px;
            flex-shrink: 0;
        }

        .tx-icon.sent {
            background: rgba(255, 59, 48, 0.1);
            color: #FF3B30;
        }
        .tx-icon.received {
            background: rgba(0, 200, 167, 0.1);
            color: #00C9A7;
        }
        .tx-icon.payment {
            background: rgba(108, 60, 225, 0.1);
            color: var(--primary);
        }
        .tx-icon.airtime {
            background: rgba(255, 152, 0, 0.1);
            color: #FF9800;
        }

        .tx-details {
            flex: 1;
        }

        .tx-name {
            font-size: 15px;
            font-weight: 600;
        }

        .tx-date {
            font-size: 12px;
            color: var(--text-light);
        }

        .tx-reference {
            font-size: 11px;
            color: var(--text-light);
            font-weight: 500;
        }

        .tx-amount {
            font-size: 16px;
            font-weight: 700;
        }

        .tx-amount.positive {
            color: #00C9A7;
        }
        .tx-amount.negative {
            color: #FF3B30;
        }

        /* Bottom Navigation */
        .bottom-nav {
            position: fixed;
            bottom: 0;
            left: 50%;
            transform: translateX(-50%);
            width: 100%;
            max-width: 430px;
            background: var(--surface);
            display: flex;
            justify-content: space-around;
            align-items: center;
            padding: 10px 0 calc(10px + var(--safe-bottom));
            border-top: 1px solid var(--border);
            z-index: 100;
            backdrop-filter: blur(10px);
            background: rgba(255, 255, 255, 0.95);
        }

        .nav-item {
            display: flex;
            flex-direction: column;
            align-items: center;
            gap: 3px;
            background: none;
            border: none;
            cursor: pointer;
            transition: var(--transition);
            padding: 4px 16px;
            position: relative;
        }

        .nav-item i {
            font-size: 22px;
            color: var(--text-light);
            transition: var(--transition);
        }

        .nav-item span {
            font-size: 10px;
            color: var(--text-light);
            font-weight: 500;
            transition: var(--transition);
        }

        .nav-item.active i {
            color: var(--primary);
        }
        .nav-item.active span {
            color: var(--primary);
        }

        .nav-item::before {
            content: '';
            position: absolute;
            top: -1px;
            left: 50%;
            transform: translateX(-50%) scaleX(0);
            width: 30px;
            height: 3px;
            background: var(--primary-gradient);
            border-radius: 0 0 3px 3px;
            transition: var(--transition);
        }

        .nav-item.active::before {
            transform: translateX(-50%) scaleX(1);
        }

        .nav-item:hover i {
            color: var(--primary);
        }

        /* Screen Headers */
        .screen-header {
            display: flex;
            align-items: center;
            gap: 15px;
            padding: 20px 20px 15px;
            background: var(--background);
            position: sticky;
            top: 0;
            z-index: 50;
            border-bottom: 1px solid var(--border);
        }

        .back-btn {
            width: 40px;
            height: 40px;
            border: none;
            background: var(--surface);
            border-radius: 50%;
            display: flex;
            align-items: center;
            justify-content: center;
            cursor: pointer;
            transition: var(--transition);
            font-size: 18px;
            box-shadow: 0 2px 10px rgba(0, 0, 0, 0.05);
        }
        .back-btn:hover {
            background: var(--primary);
            color: white;
            transform: scale(1.05);
        }

        .screen-header h2 {
            font-size: 20px;
            font-weight: 700;
        }

        /* Send / Receive / Service */
        .send-container,
        .receive-container,
        .service-container,
        .transactions-container,
        .notifications-container,
        .profile-container,
        .kyc-container {
            padding: 20px;
        }

        .recipient-select {
            display: flex;
            gap: 10px;
        }
        .recipient-select input {
            flex: 1;
        }

        .amount-input {
            position: relative;
        }

        .currency-prefix {
            position: absolute;
            left: 16px;
            top: 50%;
            transform: translateY(-50%);
            font-weight: 600;
            color: var(--text-secondary);
        }

        .amount-input input {
            padding-left: 60px;
            font-size: 20px;
            font-weight: 600;
        }

        .fee-info {
            background: var(--surface);
            padding: 16px;
            border-radius: var(--radius-sm);
            margin: 20px 0;
            border: 1px solid var(--border);
        }

        .fee-row {
            display: flex;
            justify-content: space-between;
            padding: 6px 0;
            font-size: 14px;
            color: var(--text-secondary);
        }

        .fee-row.total {
            border-top: 1px solid var(--border);
            padding-top: 12px;
            margin-top: 6px;
            font-weight: 700;
            color: var(--text-primary);
            font-size: 16px;
        }

        /* QR */
        .qr-container {
            display: flex;
            flex-direction: column;
            align-items: center;
            margin: 20px 0;
        }

        .qr-code {
            width: 250px;
            height: 250px;
            background: white;
            border-radius: var(--radius);
            display: flex;
            align-items: center;
            justify-content: center;
            box-shadow: var(--shadow);
            padding: 20px;
        }

        .qr-code .qr-placeholder {
            width: 100%;
            height: 100%;
            background: #1a1a2e;
            border-radius: 8px;
            display: flex;
            align-items: center;
            justify-content: center;
            color: white;
            font-size: 60px;
        }

        .share-info {
            width: 100%;
            margin-top: 20px;
        }

        .qr-label {
            text-align: center;
            color: var(--text-secondary);
            font-size: 14px;
            margin-bottom: 15px;
        }

        .qr-details {
            background: var(--surface);
            padding: 16px;
            border-radius: var(--radius-sm);
            border: 1px solid var(--border);
        }

        .detail-row {
            display: flex;
            justify-content: space-between;
            padding: 6px 0;
            font-size: 14px;
        }

        .detail-row .value {
            font-weight: 600;
            color: var(--text-primary);
        }

        .share-actions {
            display: flex;
            gap: 12px;
            margin-top: 20px;
        }
        .share-actions .btn-secondary {
            flex: 1;
            justify-content: center;
        }

        /* Scanner */
        .scan-container {
            padding: 20px;
        }

        .scanner-frame {
            position: relative;
            width: 100%;
            max-width: 320px;
            margin: 20px auto;
            aspect-ratio: 1;
        }

        .scanner-area {
            width: 100%;
            height: 100%;
            position: relative;
            border-radius: var(--radius);
            overflow: hidden;
            background: #1a1a2e;
        }

        .scanner-corner {
            position: absolute;
            width: 30px;
            height: 30px;
            border: 3px solid var(--primary);
        }

        .scanner-corner.tl {
            top: 15px;
            left: 15px;
            border-right: none;
            border-bottom: none;
            border-radius: 4px 0 0 0;
        }
        .scanner-corner.tr {
            top: 15px;
            right: 15px;
            border-left: none;
            border-bottom: none;
            border-radius: 0 4px 0 0;
        }
        .scanner-corner.bl {
            bottom: 15px;
            left: 15px;
            border-right: none;
            border-top: none;
            border-radius: 0 0 0 4px;
        }
        .scanner-corner.br {
            bottom: 15px;
            right: 15px;
            border-left: none;
            border-top: none;
            border-radius: 0 0 4px 0;
        }

        .scanner-line {
            position: absolute;
            top: 50%;
            left: 10%;
            width: 80%;
            height: 2px;
            background: var(--primary);
            animation: scan 2s ease-in-out infinite;
            box-shadow: 0 0 20px rgba(108, 60, 225, 0.3);
        }

        @keyframes scan {
            0%, 100% { top: 20%; }
            50% { top: 80%; }
        }

        .scanner-icon {
            position: absolute;
            top: 50%;
            left: 50%;
            transform: translate(-50%, -50%);
            font-size: 60px;
            color: rgba(255, 255, 255, 0.1);
        }

        .scan-instruction {
            text-align: center;
            color: var(--text-secondary);
            font-size: 14px;
            margin-top: 15px;
        }

        .scan-options {
            display: flex;
            gap: 12px;
            margin-top: 20px;
        }
        .scan-options .btn-secondary {
            flex: 1;
            justify-content: center;
        }

        /* Presets */
        .amount-presets {
            display: flex;
            gap: 10px;
            margin-bottom: 10px;
            flex-wrap: wrap;
        }

        .preset-btn {
            padding: 8px 20px;
            border: 2px solid var(--border);
            border-radius: var(--radius-sm);
            background: var(--surface);
            font-size: 14px;
            font-weight: 600;
            cursor: pointer;
            transition: var(--transition);
            color: var(--text-secondary);
        }
        .preset-btn:hover {
            border-color: var(--primary);
            color: var(--primary);
        }
        .preset-btn.active {
            border-color: var(--primary);
            background: var(--primary);
            color: white;
        }

        /* Bill Categories */
        .bill-categories {
            display: grid;
            grid-template-columns: repeat(4, 1fr);
            gap: 10px;
            margin-bottom: 20px;
        }

        .category-btn {
            display: flex;
            flex-direction: column;
            align-items: center;
            gap: 6px;
            padding: 12px 8px;
            border: 2px solid var(--border);
            border-radius: var(--radius-sm);
            background: var(--surface);
            cursor: pointer;
            transition: var(--transition);
            font-size: 12px;
            color: var(--text-secondary);
        }
        .category-btn i {
            font-size: 20px;
        }
        .category-btn:hover {
            border-color: var(--primary);
            color: var(--primary);
        }
        .category-btn.active {
            border-color: var(--primary);
            background: rgba(108, 60, 225, 0.05);
            color: var(--primary);
        }

        /* Service Info */
        .service-info {
            background: var(--surface);
            padding: 16px;
            border-radius: var(--radius-sm);
            margin: 20px 0;
            border: 1px solid var(--border);
        }

        .info-row {
            display: flex;
            justify-content: space-between;
            padding: 6px 0;
            font-size: 14px;
            color: var(--text-secondary);
        }

        .info-row.total {
            border-top: 1px solid var(--border);
            padding-top: 12px;
            margin-top: 6px;
            font-weight: 700;
            color: var(--text-primary);
            font-size: 16px;
        }

        /* Filter Tabs */
        .filter-tabs {
            display: flex;
            gap: 8px;
            overflow-x: auto;
            padding: 0 0 15px;
            -webkit-overflow-scrolling: touch;
            scrollbar-width: none;
        }
        .filter-tabs::-webkit-scrollbar {
            display: none;
        }

        .filter-tab {
            padding: 8px 20px;
            border: none;
            border-radius: 20px;
            background: var(--surface);
            font-size: 14px;
            font-weight: 500;
            color: var(--text-secondary);
            cursor: pointer;
            transition: var(--transition);
            white-space: nowrap;
        }
        .filter-tab.active {
            background: var(--primary-gradient);
            color: white;
            box-shadow: 0 4px 15px rgba(108, 60, 225, 0.3);
        }
        .filter-tab:hover:not(.active) {
            background: var(--border);
        }

        .transaction-list-full {
            display: flex;
            flex-direction: column;
            gap: 8px;
        }

        /* Notifications */
        .notification-item {
            display: flex;
            gap: 14px;
            padding: 16px;
            background: var(--surface);
            border-radius: var(--radius-sm);
            margin-bottom: 10px;
            transition: var(--transition);
            border-left: 3px solid transparent;
        }

        .notification-item.unread {
            border-left-color: var(--primary);
            background: rgba(108, 60, 225, 0.03);
        }
        .notification-item:hover {
            transform: translateX(4px);
        }

        .notif-icon {
            width: 44px;
            height: 44px;
            border-radius: 50%;
            display: flex;
            align-items: center;
            justify-content: center;
            background: var(--border);
            font-size: 18px;
            flex-shrink: 0;
        }

        .notif-content {
            flex: 1;
        }
        .notif-content h4 {
            font-size: 15px;
            font-weight: 600;
            margin-bottom: 4px;
        }
        .notif-content p {
            font-size: 14px;
            color: var(--text-secondary);
            line-height: 1.4;
            margin-bottom: 4px;
        }
        .notif-time {
            font-size: 12px;
            color: var(--text-light);
        }

        /* Profile */
        .profile-header {
            text-align: center;
            padding: 20px 0;
            background: var(--surface);
            border-radius: var(--radius);
            margin-bottom: 20px;
            box-shadow: var(--shadow);
        }

        .profile-avatar {
            width: 80px;
            height: 80px;
            border-radius: 50%;
            background: var(--primary-gradient);
            display: flex;
            align-items: center;
            justify-content: center;
            color: white;
            margin: 0 auto 15px;
            box-shadow: 0 10px 30px rgba(108, 60, 225, 0.3);
        }

        .profile-header h3 {
            font-size: 22px;
            font-weight: 700;
        }
        .profile-header p {
            color: var(--text-secondary);
            font-size: 14px;
            margin: 4px 0 10px;
        }

        .profile-badge {
            display: inline-block;
            padding: 4px 16px;
            background: rgba(0, 200, 167, 0.1);
            color: var(--secondary);
            border-radius: 20px;
            font-size: 12px;
            font-weight: 600;
        }

        .profile-menu {
            background: var(--surface);
            border-radius: var(--radius);
            overflow: hidden;
            box-shadow: var(--shadow);
        }

        .menu-item {
            display: flex;
            align-items: center;
            gap: 14px;
            padding: 16px 20px;
            border-bottom: 1px solid var(--border);
            cursor: pointer;
            transition: var(--transition);
        }
        .menu-item:last-child {
            border-bottom: none;
        }
        .menu-item:hover {
            background: rgba(108, 60, 225, 0.03);
        }

        .menu-item i:first-child {
            width: 20px;
            color: var(--text-secondary);
            font-size: 18px;
        }
        .menu-item span {
            flex: 1;
            font-size: 15px;
            font-weight: 500;
        }
        .menu-item i:last-child {
            color: var(--text-light);
            font-size: 14px;
        }

        .menu-item.logout {
            color: #FF3B30;
        }
        .menu-item.logout i:first-child {
            color: #FF3B30;
        }

        /* KYC */
        .kyc-status {
            text-align: center;
            padding: 30px;
            background: var(--surface);
            border-radius: var(--radius);
            margin-bottom: 20px;
            box-shadow: var(--shadow);
        }

        .status-badge {
            display: inline-flex;
            align-items: center;
            gap: 8px;
            padding: 8px 20px;
            border-radius: 20px;
            font-weight: 600;
            margin-bottom: 10px;
        }
        .status-badge.verified {
            background: rgba(0, 200, 167, 0.1);
            color: var(--secondary);
        }
        .status-badge i {
            font-size: 18px;
        }

        .kyc-info {
            background: var(--surface);
            border-radius: var(--radius);
            padding: 20px;
            margin-bottom: 20px;
            box-shadow: var(--shadow);
        }

        .kyc-info .info-item {
            display: flex;
            justify-content: space-between;
            padding: 10px 0;
            border-bottom: 1px solid var(--border);
        }
        .kyc-info .info-item:last-child {
            border-bottom: none;
        }
        .kyc-info .info-item span:first-child {
            color: var(--text-secondary);
            font-size: 14px;
        }
        .kyc-info .info-item span:last-child {
            font-weight: 600;
        }

        .kyc-levels {
            background: var(--surface);
            border-radius: var(--radius);
            padding: 20px;
            box-shadow: var(--shadow);
        }
        .kyc-levels h4 {
            margin-bottom: 15px;
        }

        .limit-item {
            display: flex;
            justify-content: space-between;
            padding: 10px 0;
            border-bottom: 1px solid var(--border);
        }
        .limit-item:last-child {
            border-bottom: none;
        }
        .limit-item span:first-child {
            color: var(--text-secondary);
            font-size: 14px;
        }
        .limit-item span:last-child {
            font-weight: 600;
        }

        /* Modal */
        .modal {
            display: none;
            position: fixed;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            background: rgba(0, 0, 0, 0.5);
            z-index: 1000;
            align-items: center;
            justify-content: center;
            backdrop-filter: blur(8px);
        }

        .modal.active {
            display: flex;
        }

        .modal-content {
            background: white;
            border-radius: var(--radius);
            max-width: 380px;
            width: 90%;
            overflow: hidden;
            animation: modalSlideUp 0.3s ease;
        }

        @keyframes modalSlideUp {
            from {
                transform: translateY(30px) scale(0.95);
                opacity: 0;
            }
            to {
                transform: translateY(0) scale(1);
                opacity: 1;
            }
        }

        .modal-header {
            padding: 20px 24px 10px;
            display: flex;
            justify-content: space-between;
            align-items: center;
            border-bottom: 1px solid var(--border);
        }
        .modal-header h3 {
            font-size: 20px;
            font-weight: 700;
        }

        .close-btn {
            width: 32px;
            height: 32px;
            border: none;
            background: var(--border);
            border-radius: 50%;
            font-size: 20px;
            cursor: pointer;
            transition: var(--transition);
            display: flex;
            align-items: center;
            justify-content: center;
        }
        .close-btn:hover {
            background: #FF3B30;
            color: white;
        }

        .modal-body {
            padding: 24px;
        }
        .modal-body p {
            font-size: 16px;
            color: var(--text-secondary);
            line-height: 1.6;
            white-space: pre-line;
        }

        .modal-footer {
            padding: 16px 24px 24px;
        }
        .modal-footer .btn-primary {
            width: 100%;
        }

        /* Responsive */
        @media (max-width: 430px) {
            #app { max-width: 100%; }
            .balance-amount { font-size: 30px; }
            .quick-actions { grid-template-columns: repeat(4, 1fr); gap: 8px; }
            .services-grid { grid-template-columns: repeat(4, 1fr); }
            .bill-categories { grid-template-columns: repeat(2, 1fr); }
        }

        @media (min-width: 431px) {
            #app {
                border-left: 1px solid var(--border);
                border-right: 1px solid var(--border);
                box-shadow: 0 0 40px rgba(0, 0, 0, 0.1);
            }
            .bottom-nav { max-width: 430px; }
        }

        /* Dark Mode */
        @media (prefers-color-scheme: dark) {
            :root {
                --background: #0F0F1A;
                --surface: #1A1A2E;
                --text-primary: #FFFFFF;
                --text-secondary: #A0A0B8;
                --text-light: #6B6B85;
                --border: #2A2A3E;
                --shadow: 0 4px 20px rgba(0, 0, 0, 0.4);
            }

            .balance-card {
                background: var(--primary-gradient);
            }

            .bottom-nav {
                background: rgba(26, 26, 46, 0.95);
            }

            .transaction-item,
            .filter-tab,
            .notification-item,
            .profile-header,
            .profile-menu,
            .kyc-status,
            .kyc-info,
            .kyc-levels {
                background: var(--surface);
            }

            .notification-item.unread {
                background: rgba(108, 60, 225, 0.08);
            }

            .modal-content {
                background: var(--surface);
            }

            .qr-code {
                background: white;
            }
        }
    </style>
</head>
<body>
    <div id="app">
        <!-- Splash Screen -->
        <div id="splash-screen" class="screen active">
            <div class="splash-content">
                <div class="splash-logo">
                    <i class="fas fa-wallet"></i>
                    <span>MYBIRR</span>
                </div>
                <div class="loader"></div>
                <p class="splash-text">Your Digital Wallet</p>
            </div>
        </div>

        <!-- Onboarding -->
        <div id="onboarding-screen" class="screen">
            <div class="onboarding-container">
                <div class="onboarding-slides">
                    <div class="slide active" data-slide="0">
                        <div class="slide-icon"><i class="fas fa-paper-plane"></i></div>
                        <h2>Send Money Instantly</h2>
                        <p>Send money to anyone in Ethiopia with just a few taps</p>
                    </div>
                    <div class="slide" data-slide="1">
                        <div class="slide-icon"><i class="fas fa-store"></i></div>
                        <h2>Pay Anywhere</h2>
                        <p>Pay merchants, buy airtime, and settle bills easily</p>
                    </div>
                    <div class="slide" data-slide="2">
                        <div class="slide-icon"><i class="fas fa-piggy-bank"></i></div>
                        <h2>Save & Grow</h2>
                        <p>Save money and access financial services</p>
                    </div>
                    <div class="slide" data-slide="3">
                        <div class="slide-icon"><i class="fas fa-shield-alt"></i></div>
                        <h2>Secure & Trusted</h2>
                        <p>Your money is protected with advanced security</p>
                    </div>
                </div>
                <div class="onboarding-dots">
                    <span class="dot active" data-slide="0"></span>
                    <span class="dot" data-slide="1"></span>
                    <span class="dot" data-slide="2"></span>
                    <span class="dot" data-slide="3"></span>
                </div>
                <div class="onboarding-buttons">
                    <button class="btn-skip" onclick="skipOnboarding()">Skip</button>
                    <button class="btn-next" onclick="nextSlide()">Next</button>
                </div>
            </div>
        </div>

        <!-- Login -->
        <div id="login-screen" class="screen">
            <div class="auth-container">
                <div class="auth-header">
                    <div class="auth-logo"><i class="fas fa-wallet"></i><span>MYBIRR</span></div>
                    <h1>Welcome Back</h1>
                    <p>Sign in to your account</p>
                </div>
                <form onsubmit="handleLogin(event)">
                    <div class="form-group">
                        <label>Phone Number</label>
                        <div class="input-group">
                            <span class="input-prefix">+251</span>
                            <input type="tel" id="login-phone" placeholder="9XXXXXXXX" required>
                        </div>
                    </div>
                    <div class="form-group">
                        <label>Password</label>
                        <div class="input-group">
                            <input type="password" id="login-password" placeholder="Enter your password" required>
                            <span class="input-icon toggle-password" onclick="togglePassword('login-password')">
                                <i class="fas fa-eye"></i>
                            </span>
                        </div>
                    </div>
                    <div class="form-options">
                        <label class="checkbox-label"><input type="checkbox" checked> Remember me</label>
                        <a href="#" onclick="showScreen('forgot-pin-screen')">Forgot PIN?</a>
                    </div>
                    <button type="submit" class="btn-primary btn-full">Sign In</button>
                </form>
                <div class="auth-footer">
                    <p>Don't have an account? <a href="#" onclick="showScreen('register-screen')">Create Account</a></p>
                </div>
            </div>
        </div>

        <!-- Register -->
        <div id="register-screen" class="screen">
            <div class="auth-container">
                <div class="auth-header">
                    <div class="auth-logo"><i class="fas fa-wallet"></i><span>MYBIRR</span></div>
                    <h1>Create Account</h1>
                    <p>Join the MYBIRR community</p>
                </div>
                <form onsubmit="handleRegister(event)">
                    <div class="form-group">
                        <label>Full Name</label>
                        <input type="text" id="register-fullname" placeholder="Enter your full name" required>
                    </div>
                    <div class="form-group">
                        <label>Phone Number</label>
                        <div class="input-group">
                            <span class="input-prefix">+251</span>
                            <input type="tel" id="register-phone" placeholder="9XXXXXXXX" required>
                        </div>
                    </div>
                    <div class="form-group">
                        <label>Email (Optional)</label>
                        <input type="email" id="register-email" placeholder="Enter your email">
                    </div>
                    <div class="form-group">
                        <label>Password</label>
                        <div class="input-group">
                            <input type="password" id="register-password" placeholder="Create a password" required>
                            <span class="input-icon toggle-password" onclick="togglePassword('register-password')">
                                <i class="fas fa-eye"></i>
                            </span>
                        </div>
                    </div>
                    <div class="form-group">
                        <label>PIN (4 digits)</label>
                        <input type="password" id="register-pin" placeholder="Enter 4-digit PIN" maxlength="4" required>
                    </div>
                    <button type="submit" class="btn-primary btn-full">Create Account</button>
                </form>
                <div class="auth-footer">
                    <p>Already have an account? <a href="#" onclick="showScreen('login-screen')">Sign In</a></p>
                </div>
            </div>
        </div>

        <!-- OTP -->
        <div id="otp-screen" class="screen">
            <div class="auth-container">
                <div class="auth-header">
                    <div class="auth-logo"><i class="fas fa-wallet"></i><span>MYBIRR</span></div>
                    <h1>Verify Your Phone</h1>
                    <p>We sent a 6-digit code to <span id="otp-phone">+251 912 345 678</span></p>
                </div>
                <form onsubmit="handleOTP(event)">
                    <div class="otp-inputs">
                        <input type="text" maxlength="1" class="otp-input" required>
                        <input type="text" maxlength="1" class="otp-input" required>
                        <input type="text" maxlength="1" class="otp-input" required>
                        <input type="text" maxlength="1" class="otp-input" required>
                        <input type="text" maxlength="1" class="otp-input" required>
                        <input type="text" maxlength="1" class="otp-input" required>
                    </div>
                    <button type="submit" class="btn-primary btn-full">Verify</button>
                </form>
                <div class="auth-footer">
                    <p>Didn't receive the code? <a href="#" onclick="resendOTP()">Resend</a></p>
                </div>
            </div>
        </div>

        <!-- Home -->
        <div id="home-screen" class="screen">
            <div class="app-header">
                <div class="header-left">
                    <div class="app-logo"><i class="fas fa-wallet"></i><span>MYBIRR</span></div>
                </div>
                <div class="header-right">
                    <button class="icon-btn" onclick="showScreen('notifications-screen')">
                        <i class="fas fa-bell"></i>
                        <span class="badge">3</span>
                    </button>
                    <div class="user-avatar" onclick="showScreen('profile-screen')"><i class="fas fa-user"></i></div>
                </div>
            </div>

            <div class="balance-card">
                <div class="balance-header">
                    <span class="balance-label">Available Balance</span>
                    <button class="icon-btn" onclick="toggleBalance()">
                        <i class="fas fa-eye" id="balance-toggle"></i>
                    </button>
                </div>
                <div class="balance-amount">
                    <span class="currency">ETB</span>
                    <span class="amount" id="balance-amount">12,450.00</span>
                </div>
                <div class="balance-details">
                    <div class="balance-item">
                        <span class="label">Account Number</span>
                        <span class="value">MYB-2024-001</span>
                    </div>
                    <div class="balance-item">
                        <span class="label">KYC Level</span>
                        <span class="value badge-verified">Verified</span>
                    </div>
                </div>
            </div>

            <div class="quick-actions">
                <button class="action-btn" onclick="showScreen('send-screen')">
                    <div class="action-icon send"><i class="fas fa-paper-plane"></i></div>
                    <span>Send</span>
                </button>
                <button class="action-btn" onclick="showScreen('receive-screen')">
                    <div class="action-icon receive"><i class="fas fa-download"></i></div>
                    <span>Receive</span>
                </button>
                <button class="action-btn" onclick="showScreen('scan-screen')">
                    <div class="action-icon scan"><i class="fas fa-qrcode"></i></div>
                    <span>Scan</span>
                </button>
                <button class="action-btn" onclick="showScreen('pay-screen')">
                    <div class="action-icon pay"><i class="fas fa-store"></i></div>
                    <span>Pay</span>
                </button>
            </div>

            <div class="services-grid">
                <button class="service-btn" onclick="showScreen('airtime-screen')">
                    <i class="fas fa-phone"></i><span>Airtime</span>
                </button>
                <button class="service-btn" onclick="showScreen('data-screen')">
                    <i class="fas fa-wifi"></i><span>Data</span>
                </button>
                <button class="service-btn" onclick="showScreen('bills-screen')">
                    <i class="fas fa-file-invoice"></i><span>Bills</span>
                </button>
                <button class="service-btn" onclick="showScreen('savings-screen')">
                    <i class="fas fa-piggy-bank"></i><span>Savings</span>
                </button>
            </div>

            <div class="recent-transactions">
                <div class="section-header">
                    <h3>Recent Transactions</h3>
                    <a href="#" onclick="showScreen('transactions-screen')">View All</a>
                </div>
                <div class="transaction-list">
                    <div class="transaction-item">
                        <div class="tx-icon sent"><i class="fas fa-arrow-up"></i></div>
                        <div class="tx-details">
                            <div class="tx-name">Sent to Abebe B.</div>
                            <div class="tx-date">Today, 14:30</div>
                        </div>
                        <div class="tx-amount negative">-ETB 500.00</div>
                    </div>
                    <div class="transaction-item">
                        <div class="tx-icon received"><i class="fas fa-arrow-down"></i></div>
                        <div class="tx-details">
                            <div class="tx-name">Received from Alem M.</div>
                            <div class="tx-date">Today, 12:15</div>
                        </div>
                        <div class="tx-amount positive">+ETB 1,000.00</div>
                    </div>
                    <div class="transaction-item">
                        <div class="tx-icon payment"><i class="fas fa-shopping-bag"></i></div>
                        <div class="tx-details">
                            <div class="tx-name">Payment to ABC Store</div>
                            <div class="tx-date">Yesterday, 18:45</div>
                        </div>
                        <div class="tx-amount negative">-ETB 250.00</div>
                    </div>
                    <div class="transaction-item">
                        <div class="tx-icon airtime"><i class="fas fa-phone-alt"></i></div>
                        <div class="tx-details">
                            <div class="tx-name">Airtime Purchase</div>
                            <div class="tx-date">Yesterday, 10:20</div>
                        </div>
                        <div class="tx-amount negative">-ETB 50.00</div>
                    </div>
                </div>
            </div>

            <div class="bottom-nav">
                <button class="nav-item active" onclick="showScreen('home-screen')">
                    <i class="fas fa-home"></i><span>Home</span>
                </button>
                <button class="nav-item" onclick="showScreen('transactions-screen')">
                    <i class="fas fa-clock"></i><span>History</span>
                </button>
                <button class="nav-item" onclick="showScreen('scan-screen')">
                    <i class="fas fa-qrcode"></i><span>Scan</span>
                </button>
                <button class="nav-item" onclick="showScreen('profile-screen')">
                    <i class="fas fa-user"></i><span>Profile</span>
                </button>
            </div>
        </div>

        <!-- Send Money -->
        <div id="send-screen" class="screen">
            <div class="screen-header">
                <button class="back-btn" onclick="showScreen('home-screen')"><i class="fas fa-arrow-left"></i></button>
                <h2>Send Money</h2>
            </div>
            <div class="send-container">
                <div class="form-group">
                    <label>Recipient</label>
                    <div class="recipient-select">
                        <input type="text" id="recipient-input" placeholder="Phone number or select contact">
                        <button class="icon-btn"><i class="fas fa-address-book"></i></button>
                    </div>
                </div>
                <div class="form-group">
                    <label>Amount (ETB)</label>
                    <div class="amount-input">
                        <span class="currency-prefix">ETB</span>
                        <input type="number" id="send-amount" placeholder="0.00" step="0.01">
                    </div>
                </div>
                <div class="form-group">
                    <label>Description (Optional)</label>
                    <input type="text" id="send-description" placeholder="What's this for?">
                </div>
                <div class="fee-info">
                    <div class="fee-row"><span>Transfer Fee</span><span>ETB 5.00</span></div>
                    <div class="fee-row total"><span>Total</span><span>ETB 505.00</span></div>
                </div>
                <button class="btn-primary btn-full" onclick="processSendMoney()">Send Money</button>
            </div>
        </div>

        <!-- Receive Money -->
        <div id="receive-screen" class="screen">
            <div class="screen-header">
                <button class="back-btn" onclick="showScreen('home-screen')"><i class="fas fa-arrow-left"></i></button>
                <h2>Receive Money</h2>
            </div>
            <div class="receive-container">
                <div class="qr-container">
                    <div class="qr-code">
                        <div class="qr-placeholder"><i class="fas fa-qrcode"></i></div>
                    </div>
                    <div class="share-info">
                        <p class="qr-label">Scan to receive money</p>
                        <div class="qr-details">
                            <div class="detail-row"><span>MYBIRR ID</span><span class="value">MYB-2024-001</span></div>
                            <div class="detail-row"><span>Phone</span><span class="value">+251 912 345 678</span></div>
                        </div>
                    </div>
                </div>
                <div class="share-actions">
                    <button class="btn-secondary" onclick="shareQR()"><i class="fas fa-share-alt"></i> Share QR</button>
                    <button class="btn-secondary" onclick="copyInfo()"><i class="fas fa-copy"></i> Copy</button>
                </div>
            </div>
        </div>

        <!-- Scan QR -->
        <div id="scan-screen" class="screen">
            <div class="screen-header">
                <button class="back-btn" onclick="showScreen('home-screen')"><i class="fas fa-arrow-left"></i></button>
                <h2>Scan QR</h2>
            </div>
            <div class="scan-container">
                <div class="scanner-frame">
                    <div class="scanner-area">
                        <div class="scanner-corner tl"></div>
                        <div class="scanner-corner tr"></div>
                        <div class="scanner-corner bl"></div>
                        <div class="scanner-corner br"></div>
                        <div class="scanner-line"></div>
                        <i class="fas fa-camera scanner-icon"></i>
                    </div>
                    <p class="scan-instruction">Position QR code within the frame</p>
                </div>
                <div class="scan-options">
                    <button class="btn-secondary" onclick="uploadQR()"><i class="fas fa-image"></i> Upload</button>
                    <button class="btn-secondary" onclick="flashToggle()"><i class="fas fa-bolt"></i> Flash</button>
                </div>
            </div>
        </div>

        <!-- Pay Merchant -->
        <div id="pay-screen" class="screen">
            <div class="screen-header">
                <button class="back-btn" onclick="showScreen('home-screen')"><i class="fas fa-arrow-left"></i></button>
                <h2>Pay Merchant</h2>
            </div>
            <div class="send-container">
                <div class="form-group">
                    <label>Merchant</label>
                    <input type="text" id="merchant-input" placeholder="Enter merchant name or scan QR">
                </div>
                <div class="form-group">
                    <label>Amount (ETB)</label>
                    <div class="amount-input">
                        <span class="currency-prefix">ETB</span>
                        <input type="number" id="pay-amount" placeholder="0.00" step="0.01">
                    </div>
                </div>
                <div class="form-group">
                    <label>Reference (Optional)</label>
                    <input type="text" id="pay-reference" placeholder="Order or invoice number">
                </div>
                <div class="fee-info">
                    <div class="fee-row"><span>Service Fee</span><span>ETB 0.00</span></div>
                    <div class="fee-row total"><span>Total</span><span>ETB 0.00</span></div>
                </div>
                <button class="btn-primary btn-full" onclick="processPayment()">Pay Now</button>
            </div>
        </div>

        <!-- Airtime -->
        <div id="airtime-screen" class="screen">
            <div class="screen-header">
                <button class="back-btn" onclick="showScreen('home-screen')"><i class="fas fa-arrow-left"></i></button>
                <h2>Buy Airtime</h2>
            </div>
            <div class="service-container">
                <div class="form-group">
                    <label>Phone Number</label>
                    <input type="tel" id="airtime-phone" placeholder="Enter phone number">
                </div>
                <div class="form-group">
                    <label>Amount (ETB)</label>
                    <div class="amount-presets">
                        <button class="preset-btn" onclick="setAirtimeAmount(10)">10</button>
                        <button class="preset-btn" onclick="setAirtimeAmount(25)">25</button>
                        <button class="preset-btn" onclick="setAirtimeAmount(50)">50</button>
                        <button class="preset-btn" onclick="setAirtimeAmount(100)">100</button>
                    </div>
                    <input type="number" id="airtime-amount" placeholder="Enter amount" step="1">
                </div>
                <div class="service-info">
                    <div class="info-row"><span>Provider</span><span>Ethio Telecom</span></div>
                    <div class="info-row total"><span>Total</span><span>ETB <span id="airtime-total">0.00</span></span></div>
                </div>
                <button class="btn-primary btn-full" onclick="processAirtime()">Buy Airtime</button>
            </div>
        </div>

        <!-- Data -->
        <div id="data-screen" class="screen">
            <div class="screen-header">
                <button class="back-btn" onclick="showScreen('home-screen')"><i class="fas fa-arrow-left"></i></button>
                <h2>Buy Data</h2>
            </div>
            <div class="service-container">
                <div class="form-group">
                    <label>Phone Number</label>
                    <input type="tel" id="data-phone" placeholder="Enter phone number">
                </div>
                <div class="form-group">
                    <label>Data Package</label>
                    <select id="data-package">
                        <option value="100">100 MB - ETB 15</option>
                        <option value="500">500 MB - ETB 50</option>
                        <option value="1000">1 GB - ETB 80</option>
                        <option value="2000">2 GB - ETB 150</option>
                        <option value="5000">5 GB - ETB 300</option>
                    </select>
                </div>
                <div class="service-info">
                    <div class="info-row"><span>Provider</span><span>Ethio Telecom</span></div>
                    <div class="info-row total"><span>Total</span><span>ETB <span id="data-total">0.00</span></span></div>
                </div>
                <button class="btn-primary btn-full" onclick="processData()">Buy Data</button>
            </div>
        </div>

        <!-- Bills -->
        <div id="bills-screen" class="screen">
            <div class="screen-header">
                <button class="back-btn" onclick="showScreen('home-screen')"><i class="fas fa-arrow-left"></i></button>
                <h2>Pay Bills</h2>
            </div>
            <div class="service-container">
                <div class="bill-categories">
                    <button class="category-btn active" onclick="selectBillCategory(this, 'electricity')">
                        <i class="fas fa-bolt"></i><span>Electricity</span>
                    </button>
                    <button class="category-btn" onclick="selectBillCategory(this, 'water')">
                        <i class="fas fa-water"></i><span>Water</span>
                    </button>
                    <button class="category-btn" onclick="selectBillCategory(this, 'internet')">
                        <i class="fas fa-wifi"></i><span>Internet</span>
                    </button>
                    <button class="category-btn" onclick="selectBillCategory(this, 'tv')">
                        <i class="fas fa-tv"></i><span>TV</span>
                    </button>
                </div>
                <div class="form-group">
                    <label>Customer Number</label>
                    <input type="text" id="bill-customer" placeholder="Enter customer number">
                </div>
                <div class="form-group">
                    <label>Amount (ETB)</label>
                    <input type="number" id="bill-amount" placeholder="Enter amount">
                </div>
                <div class="service-info">
                    <div class="info-row"><span>Service Fee</span><span>ETB 0.00</span></div>
                    <div class="info-row total"><span>Total</span><span>ETB <span id="bill-total">0.00</span></span></div>
                </div>
                <button class="btn-primary btn-full" onclick="processBill()">Pay Bill</button>
            </div>
        </div>

        <!-- Savings -->
        <div id="savings-screen" class="screen">
            <div class="screen-header">
                <button class="back-btn" onclick="showScreen('home-screen')"><i class="fas fa-arrow-left"></i></button>
                <h2>Savings</h2>
            </div>
            <div class="service-container">
                <div class="balance-card" style="margin:0 0 20px 0;">
                    <div class="balance-header">
                        <span class="balance-label">Savings Balance</span>
                    </div>
                    <div class="balance-amount">
                        <span class="currency">ETB</span>
                        <span class="amount">3,250.00</span>
                    </div>
                </div>
                <div class="form-group">
                    <label>Amount to Save</label>
                    <div class="amount-input">
                        <span class="currency-prefix">ETB</span>
                        <input type="number" id="savings-amount" placeholder="0.00" step="0.01">
                    </div>
                </div>
                <div class="form-group">
                    <label>Goal (Optional)</label>
                    <input type="text" id="savings-goal" placeholder="e.g., Emergency Fund">
                </div>
                <button class="btn-primary btn-full" onclick="processSavings()">Save Money</button>
            </div>
        </div>

        <!-- Transactions -->
        <div id="transactions-screen" class="screen">
            <div class="screen-header">
                <button class="back-btn" onclick="showScreen('home-screen')"><i class="fas fa-arrow-left"></i></button>
                <h2>Transactions</h2>
            </div>
            <div class="transactions-container">
                <div class="filter-tabs">
                    <button class="filter-tab active" onclick="filterTransactions(this, 'all')">All</button>
                    <button class="filter-tab" onclick="filterTransactions(this, 'sent')">Sent</button>
                    <button class="filter-tab" onclick="filterTransactions(this, 'received')">Received</button>
                    <button class="filter-tab" onclick="filterTransactions(this, 'payments')">Payments</button>
                </div>
                <div class="transaction-list-full">
                    <div class="transaction-item">
                        <div class="tx-icon sent"><i class="fas fa-arrow-up"></i></div>
                        <div class="tx-details">
                            <div class="tx-name">Sent to Abebe B.</div>
                            <div class="tx-date">Today, 14:30</div>
                            <div class="tx-reference">REF: MYB-2024-001</div>
                        </div>
                        <div class="tx-amount negative">-ETB 500.00</div>
                    </div>
                    <div class="transaction-item">
                        <div class="tx-icon received"><i class="fas fa-arrow-down"></i></div>
                        <div class="tx-details">
                            <div class="tx-name">Received from Alem M.</div>
                            <div class="tx-date">Today, 12:15</div>
                            <div class="tx-reference">REF: MYB-2024-002</div>
                        </div>
                        <div class="tx-amount positive">+ETB 1,000.00</div>
                    </div>
                    <div class="transaction-item">
                        <div class="tx-icon payment"><i class="fas fa-shopping-bag"></i></div>
                        <div class="tx-details">
                            <div class="tx-name">Payment to ABC Store</div>
                            <div class="tx-date">Yesterday, 18:45</div>
                            <div class="tx-reference">REF: MYB-2024-003</div>
                        </div>
                        <div class="tx-amount negative">-ETB 250.00</div>
                    </div>
                    <div class="transaction-item">
                        <div class="tx-icon airtime"><i class="fas fa-phone-alt"></i></div>
                        <div class="tx-details">
                            <div class="tx-name">Airtime Purchase</div>
                            <div class="tx-date">Yesterday, 10:20</div>
                            <div class="tx-reference">REF: MYB-2024-004</div>
                        </div>
                        <div class="tx-amount negative">-ETB 50.00</div>
                    </div>
                </div>
            </div>
        </div>

        <!-- Notifications -->
        <div id="notifications-screen" class="screen">
            <div class="screen-header">
                <button class="back-btn" onclick="showScreen('home-screen')"><i class="fas fa-arrow-left"></i></button>
                <h2>Notifications</h2>
            </div>
            <div class="notifications-container">
                <div class="notification-item unread">
                    <div class="notif-icon"><i class="fas fa-check-circle" style="color:#00c853;"></i></div>
                    <div class="notif-content">
                        <h4>Payment Received</h4>
                        <p>You received ETB 1,000.00 from Alem M.</p>
                        <span class="notif-time">2 minutes ago</span>
                    </div>
                </div>
                <div class="notification-item unread">
                    <div class="notif-icon"><i class="fas fa-exclamation-circle" style="color:#ff9800;"></i></div>
                    <div class="notif-content">
                        <h4>Security Alert</h4>
                        <p>New login detected from Addis Ababa</p>
                        <span class="notif-time">1 hour ago</span>
                    </div>
                </div>
                <div class="notification-item">
                    <div class="notif-icon"><i class="fas fa-gift" style="color:#e91e63;"></i></div>
                    <div class="notif-content">
                        <h4>Promotion</h4>
                        <p>Get 5% cashback on your next transfer</p>
                        <span class="notif-time">3 hours ago</span>
                    </div>
                </div>
            </div>
        </div>

        <!-- Profile -->
        <div id="profile-screen" class="screen">
            <div class="screen-header">
                <button class="back-btn" onclick="showScreen('home-screen')"><i class="fas fa-arrow-left"></i></button>
                <h2>Profile</h2>
            </div>
            <div class="profile-container">
                <div class="profile-header">
                    <div class="profile-avatar"><i class="fas fa-user" style="font-size:60px;"></i></div>
                    <h3>Getachew T.</h3>
                    <p>+251 912 345 678</p>
                    <div class="profile-badge">Verified</div>
                </div>
                <div class="profile-menu">
                    <div class="menu-item" onclick="showScreen('kyc-screen')">
                        <i class="fas fa-id-card"></i><span>KYC Verification</span><i class="fas fa-chevron-right"></i>
                    </div>
                    <div class="menu-item">
                        <i class="fas fa-shield-alt"></i><span>Security Settings</span><i class="fas fa-chevron-right"></i>
                    </div>
                    <div class="menu-item">
                        <i class="fas fa-language"></i><span>Language</span><i class="fas fa-chevron-right"></i>
                    </div>
                    <div class="menu-item">
                        <i class="fas fa-users"></i><span>Refer & Earn</span><i class="fas fa-chevron-right"></i>
                    </div>
                    <div class="menu-item">
                        <i class="fas fa-question-circle"></i><span>Help & Support</span><i class="fas fa-chevron-right"></i>
                    </div>
                    <div class="menu-item">
                        <i class="fas fa-info-circle"></i><span>About MYBIRR</span><i class="fas fa-chevron-right"></i>
                    </div>
                    <div class="menu-item logout" onclick="handleLogout()">
                        <i class="fas fa-sign-out-alt"></i><span>Logout</span><i class="fas fa-chevron-right"></i>
                    </div>
                </div>
            </div>
        </div>

        <!-- Forgot PIN -->
        <div id="forgot-pin-screen" class="screen">
            <div class="auth-container">
                <div class="auth-header">
                    <div class="auth-logo"><i class="fas fa-wallet"></i><span>MYBIRR</span></div>
                    <h1>Reset PIN</h1>
                    <p>Enter your phone number to reset your PIN</p>
                </div>
                <form onsubmit="handleForgotPIN(event)">
                    <div class="form-group">
                        <label>Phone Number</label>
                        <div class="input-group">
                            <span class="input-prefix">+251</span>
                            <input type="tel" placeholder="9XXXXXXXX" required>
                        </div>
                    </div>
                    <button type="submit" class="btn-primary btn-full">Send Reset Code</button>
                </form>
                <div class="auth-footer">
                    <p><a href="#" onclick="showScreen('login-screen')">Back to Sign In</a></p>
                </div>
            </div>
        </div>

        <!-- KYC -->
        <div id="kyc-screen" class="screen">
            <div class="screen-header">
                <button class="back-btn" onclick="showScreen('profile-screen')"><i class="fas fa-arrow-left"></i></button>
                <h2>KYC Verification</h2>
            </div>
            <div class="kyc-container">
                <div class="kyc-status">
                    <div class="status-badge verified"><i class="fas fa-check-circle"></i> Verified</div>
                    <p>Your account is fully verified</p>
                </div>
                <div class="kyc-info">
                    <div class="info-item"><span>Full Name</span><span>Getachew T.</span></div>
                    <div class="info-item"><span>ID Type</span><span>National ID</span></div>
                    <div class="info-item"><span>ID Number</span><span>1234567890</span></div>
                    <div class="info-item"><span>Verification Date</span><span>15/01/2024</span></div>
                </div>
                <div class="kyc-levels">
                    <h4>Your Limits</h4>
                    <div class="limit-item"><span>Daily Transfer</span><span>ETB 20,000</span></div>
                    <div class="limit-item"><span>Monthly Transfer</span><span>ETB 100,000</span></div>
                    <div class="limit-item"><span>Single Transaction</span><span>ETB 10,000</span></div>
                </div>
            </div>
        </div>
    </div>

    <!-- Modal -->
    <div id="modal" class="modal">
        <div class="modal-content">
            <div class="modal-header">
                <h3 id="modal-title">Success</h3>
                <button class="close-btn" onclick="closeModal()">&times;</button>
            </div>
            <div class="modal-body">
                <p id="modal-message">Transaction completed successfully!</p>
            </div>
            <div class="modal-footer">
                <button class="btn-primary" onclick="closeModal()">OK</button>
            </div>
        </div>
    </div>

    <script>
        // ============================================
        // APP STATE
        // ============================================
        const AppState = {
            currentScreen: 'splash-screen',
            balance: 12450.00,
            isBalanceVisible: true,
            selectedSlide: 0,
            totalSlides: 4,
            user: {
                name: 'Getachew T.',
                phone: '+251 912 345 678',
                email: 'getachew@email.com',
                isVerified: true,
                kycLevel: 'verified'
            }
        };

        // ============================================
        // SCREEN MANAGEMENT
        // ============================================
        function showScreen(screenId) {
            document.querySelectorAll('.screen').forEach(screen => {
                screen.classList.remove('active');
            });
            const targetScreen = document.getElementById(screenId);
            if (targetScreen) {
                targetScreen.classList.add('active');
                AppState.currentScreen = screenId;
                targetScreen.scrollTop = 0;
            }
        }

        function showModal(title, message) {
            document.getElementById('modal-title').textContent = title;
            document.getElementById('modal-message').textContent = message;
            document.getElementById('modal').classList.add('active');
        }

        function closeModal() {
            document.getElementById('modal').classList.remove('active');
        }

        // ============================================
        // SPLASH
        // ============================================
        function initSplash() {
            setTimeout(() => {
                const isLoggedIn = localStorage.getItem('mybirr_logged_in') === 'true';
                showScreen(isLoggedIn ? 'home-screen' : 'onboarding-screen');
                if (isLoggedIn) updateBalance();
            }, 3000);
        }

        // ============================================
        // ONBOARDING
        // ============================================
        function nextSlide() {
            if (AppState.selectedSlide < AppState.totalSlides - 1) {
                AppState.selectedSlide++;
                updateSlide();
            } else {
                showScreen('login-screen');
            }
        }

        function skipOnboarding() {
            showScreen('login-screen');
        }

        function updateSlide() {
            document.querySelectorAll('.slide').forEach((slide, index) => {
                slide.classList.toggle('active', index === AppState.selectedSlide);
            });
            document.querySelectorAll('.dot').forEach((dot, index) => {
                dot.classList.toggle('active', index === AppState.selectedSlide);
            });
            const btnNext = document.querySelector('.btn-next');
            btnNext.textContent = AppState.selectedSlide === AppState.totalSlides - 1 ? 'Get Started' : 'Next';
        }

        document.querySelectorAll('.dot').forEach((dot, index) => {
            dot.addEventListener('click', () => {
                AppState.selectedSlide = index;
                updateSlide();
            });
        });

        // ============================================
        // AUTHENTICATION
        // ============================================
        function handleLogin(event) {
            event.preventDefault();
            const phone = document.getElementById('login-phone').value;
            const password = document.getElementById('login-password').value;
            if (!phone || !password) {
                showModal('Error', 'Please fill in all fields');
                return;
            }
            localStorage.setItem('mybirr_logged_in', 'true');
            showModal('Success', 'Login successful!');
            setTimeout(() => {
                closeModal();
                showScreen('home-screen');
                updateBalance();
            }, 1500);
        }

        function handleRegister(event) {
            event.preventDefault();
            const fullname = document.getElementById('register-fullname').value;
            const phone = document.getElementById('register-phone').value;
            const password = document.getElementById('register-password').value;
            const pin = document.getElementById('register-pin').value;
            if (!fullname || !phone || !password || !pin) {
                showModal('Error', 'Please fill in all fields');
                return;
            }
            if (pin.length !== 4 || !/^\d+$/.test(pin)) {
                showModal('Error', 'PIN must be 4 digits');
                return;
            }
            showModal('Success', 'Account created! Please verify your phone.');
            setTimeout(() => {
                closeModal();
                showScreen('otp-screen');
                document.getElementById('otp-phone').textContent = '+251 ' + phone;
            }, 1500);
        }

        function handleOTP(event) {
            event.preventDefault();
            const inputs = document.querySelectorAll('.otp-input');
            let otp = '';
            inputs.forEach(input => otp += input.value);
            if (otp.length !== 6) {
                showModal('Error', 'Please enter the 6-digit code');
                return;
            }
            localStorage.setItem('mybirr_logged_in', 'true');
            showModal('Success', 'Phone verified! Welcome to MYBIRR.');
            setTimeout(() => {
                closeModal();
                showScreen('home-screen');
                updateBalance();
            }, 1500);
        }

        function resendOTP() {
            showModal('Info', 'New OTP sent to your phone');
            setTimeout(closeModal, 2000);
        }

        function handleForgotPIN(event) {
            event.preventDefault();
            showModal('Info', 'PIN reset instructions sent to your phone');
            setTimeout(() => {
                closeModal();
                showScreen('login-screen');
            }, 2000);
        }

        function handleLogout() {
            if (confirm('Are you sure you want to logout?')) {
                localStorage.removeItem('mybirr_logged_in');
                showScreen('login-screen');
                showModal('Info', 'Logged out successfully');
                setTimeout(closeModal, 1500);
            }
        }

        function togglePassword(inputId) {
            const input = document.getElementById(inputId);
            const icon = input.parentElement.querySelector('.toggle-password i');
            if (input.type === 'password') {
                input.type = 'text';
                icon.className = 'fas fa-eye-slash';
            } else {
                input.type = 'password';
                icon.className = 'fas fa-eye';
            }
        }

        // ============================================
        // BALANCE
        // ============================================
        function updateBalance() {
            const balanceEl = document.getElementById('balance-amount');
            if (balanceEl) {
                const formatted = AppState.balance.toLocaleString('en-US', {
                    minimumFractionDigits: 2,
                    maximumFractionDigits: 2
                });
                balanceEl.textContent = formatted;
            }
        }

        function toggleBalance() {
            const balanceEl = document.getElementById('balance-amount');
            const toggleBtn = document.getElementById('balance-toggle');
            if (AppState.isBalanceVisible) {
                balanceEl.textContent = '••••••';
                toggleBtn.className = 'fas fa-eye-slash';
                AppState.isBalanceVisible = false;
            } else {
                updateBalance();
                toggleBtn.className = 'fas fa-eye';
                AppState.isBalanceVisible = true;
            }
        }

        // ============================================
        // TRANSACTIONS
        // ============================================
        function filterTransactions(element, type) {
            document.querySelectorAll('.filter-tab').forEach(tab => tab.classList.remove('active'));
            element.classList.add('active');
            showModal('Info', `Showing ${type} transactions`);
            setTimeout(closeModal, 1000);
        }

        // ============================================
        // SEND MONEY
        // ============================================
        function processSendMoney() {
            const recipient = document.getElementById('recipient-input').value;
            const amount = parseFloat(document.getElementById('send-amount').value);
            if (!recipient) { showModal('Error', 'Please enter recipient'); return; }
            if (!amount || amount <= 0) { showModal('Error', 'Please enter a valid amount'); return; }
            if (amount > AppState.balance) { showModal('Error', 'Insufficient balance'); return; }
            const fee = amount * 0.01;
            const total = amount + fee;
            showModal('Confirm', `Send ETB ${amount.toFixed(2)} to ${recipient}?\nFee: ETB ${fee.toFixed(2)}\nTotal: ETB ${total.toFixed(2)}`);
            setTimeout(() => {
                closeModal();
                AppState.balance -= total;
                updateBalance();
                showModal('Success', 'Transaction completed successfully!');
                setTimeout(closeModal, 2000);
            }, 2000);
        }

        // ============================================
        // PAYMENT (Merchant)
        // ============================================
        function processPayment() {
            const merchant = document.getElementById('merchant-input').value;
            const amount = parseFloat(document.getElementById('pay-amount').value);
            if (!merchant) { showModal('Error', 'Please enter merchant name'); return; }
            if (!amount || amount <= 0) { showModal('Error', 'Please enter a valid amount'); return; }
            if (amount > AppState.balance) { showModal('Error', 'Insufficient balance'); return; }
            showModal('Confirm', `Pay ETB ${amount.toFixed(2)} to ${merchant}?`);
            setTimeout(() => {
                closeModal();
                AppState.balance -= amount;
                updateBalance();
                showModal('Success', 'Payment completed!');
                setTimeout(closeModal, 2000);
            }, 2000);
        }

        // ============================================
        // AIRTIME
        // ============================================
        function setAirtimeAmount(amount) {
            document.getElementById('airtime-amount').value = amount;
            updateAirtimeTotal();
            document.querySelectorAll('.preset-btn').forEach(btn => {
                btn.classList.toggle('active', parseInt(btn.textContent) === amount);
            });
        }

        function updateAirtimeTotal() {
            const amount = parseFloat(document.getElementById('airtime-amount').value) || 0;
            document.getElementById('airtime-total').textContent = amount.toFixed(2);
        }

        document.getElementById('airtime-amount').addEventListener('input', updateAirtimeTotal);

        function processAirtime() {
            const phone = document.getElementById('airtime-phone').value;
            const amount = parseFloat(document.getElementById('airtime-amount').value);
            if (!phone) { showModal('Error', 'Please enter phone number'); return; }
            if (!amount || amount <= 0) { showModal('Error', 'Please enter amount'); return; }
            if (amount > AppState.balance) { showModal('Error', 'Insufficient balance'); return; }
            AppState.balance -= amount;
            updateBalance();
            showModal('Success', `Airtime of ETB ${amount.toFixed(2)} sent to ${phone}`);
            setTimeout(closeModal, 2000);
        }

        // ============================================
        // DATA
        // ============================================
        function updateDataTotal() {
            const select = document.getElementById('data-package');
            const amount = parseFloat(select.options[select.selectedIndex].text.match(/ETB (\d+)/)?.[1] || 0);
            document.getElementById('data-total').textContent = amount.toFixed(2);
        }

        document.getElementById('data-package').addEventListener('change', updateDataTotal);

        function processData() {
            const phone = document.getElementById('data-phone').value;
            const select = document.getElementById('data-package');
            const packageText = select.options[select.selectedIndex].text;
            const amount = parseFloat(packageText.match(/ETB (\d+)/)?.[1] || 0);
            if (!phone) { showModal('Error', 'Please enter phone number'); return; }
            if (amount > AppState.balance) { showModal('Error', 'Insufficient balance'); return; }
            AppState.balance -= amount;
            updateBalance();
            showModal('Success', `Data package (${packageText}) purchased for ${phone}`);
            setTimeout(closeModal, 2000);
        }

        // ============================================
        // BILLS
        // ============================================
        function selectBillCategory(element, category) {
            document.querySelectorAll('.category-btn').forEach(btn => btn.classList.remove('active'));
            element.classList.add('active');
            updateBillTotal();
        }

        function updateBillTotal() {
            const amount = parseFloat(document.getElementById('bill-amount').value) || 0;
            document.getElementById('bill-total').textContent = amount.toFixed(2);
        }

        document.getElementById('bill-amount').addEventListener('input', updateBillTotal);

        function processBill() {
            const customer = document.getElementById('bill-customer').value;
            const amount = parseFloat(document.getElementById('bill-amount').value);
            if (!customer) { showModal('Error', 'Please enter customer number'); return; }
            if (!amount || amount <= 0) { showModal('Error', 'Please enter amount'); return; }
            if (amount > AppState.balance) { showModal('Error', 'Insufficient balance'); return; }
            AppState.balance -= amount;
            updateBalance();
            showModal('Success', `Bill payment of ETB ${amount.toFixed(2)} completed!`);
            setTimeout(closeModal, 2000);
        }

        // ============================================
        // SAVINGS
        // ============================================
        function processSavings() {
            const amount = parseFloat(document.getElementById('savings-amount').value);
            const goal = document.getElementById('savings-goal').value || 'Savings';
            if (!amount || amount <= 0) { showModal('Error', 'Please enter amount'); return; }
            if (amount > AppState.balance) { showModal('Error', 'Insufficient balance'); return; }
            AppState.balance -= amount;
            updateBalance();
            showModal('Success', `ETB ${amount.toFixed(2)} saved for "${goal}"`);
            setTimeout(closeModal, 2000);
        }

        // ============================================
        // QR FUNCTIONS
        // ============================================
        function shareQR() {
            if (navigator.share) {
                navigator.share({
                    title: 'MYBIRR Payment QR',
                    text: 'Scan to pay me on MYBIRR',
                    url: window.location.href
                }).catch(() => {});
            } else {
                showModal('Info', 'QR Code: MYB-2024-001\nPhone: +251 912 345 678');
                setTimeout(closeModal, 3000);
            }
        }

        function copyInfo() {
            const info = 'MYBIRR ID: MYB-2024-001\nPhone: +251 912 345 678';
            navigator.clipboard.writeText(info).then(() => {
                showModal('Success', 'Details copied to clipboard!');
                setTimeout(closeModal, 1500);
            }).catch(() => {
                showModal('Info', info);
                setTimeout(closeModal, 3000);
            });
        }

        function uploadQR() {
            showModal('Info', 'QR upload feature coming soon');
            setTimeout(closeModal, 2000);
        }

        function flashToggle() {
            showModal('Info', 'Flash toggle feature coming soon');
            setTimeout(closeModal, 2000);
        }

        // ============================================
        // OTP INPUT HANDLING
        // ============================================
        document.querySelectorAll('.otp-input').forEach((input, index, inputs) => {
            input.addEventListener('input', function() {
                if (this.value.length === 1 && index < inputs.length - 1) {
                    inputs[index + 1].focus();
                }
            });
            input.addEventListener('keydown', function(e) {
                if (e.key === 'Backspace' && this.value.length === 0 && index > 0) {
                    inputs[index - 1].focus();
                }
            });
            input.addEventListener('keypress', function(e) {
                if (!/^\d$/.test(e.key)) e.preventDefault();
            });
        });

        // ============================================
        // KEYBOARD SHORTCUTS
        // ============================================
        document.addEventListener('keydown', function(e) {
            if (e.key === 'Escape') closeModal();
        });

        // ============================================
        // INITIALIZATION
        // ============================================
        document.addEventListener('DOMContentLoaded', function() {
            showScreen('splash-screen');
            initSplash();
            updateAirtimeTotal();
            updateBillTotal();
            updateDataTotal();
            console.log('🚀 MYBIRR App Initialized');
            console.log('📱 Version: 1.0.0');
            console.log('👤 User:', AppState.user.name);
        });
    </script>
</body>
</html>