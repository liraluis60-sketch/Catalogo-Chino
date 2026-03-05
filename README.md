<!DOCTYPE html>
<html lang="es">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
    <meta name="format-detection" content="telephone=no">
    <title>ZHUO - Catálogo Interactivo</title>
    <link href="https://fonts.googleapis.com/css2?family=Ubuntu:wght@300;400;500;700&family=Noto+Sans+JP:wght@300;400;700&display=swap" rel="stylesheet">
    <style>
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
            -webkit-tap-highlight-color: transparent;
        }

        :root {
            --bg-dark: #0a0a0a;
            --bg-card: #151515;
            --bg-hover: #1f1f1f;
            --red-accent: #d5222c;
            --red-hover: #ff2a35;
            --text-white: #ffffff;
            --text-gray: #a0a0a0;
            --text-light: #d5c3be;
            --border: rgba(255, 255, 255, 0.1);
        }

        body {
            font-family: 'Ubuntu', -apple-system, BlinkMacSystemFont, sans-serif;
            background-color: var(--bg-dark);
            color: var(--text-white);
            overflow-x: hidden;
            line-height: 1.6;
            -webkit-font-smoothing: antialiased;
            -moz-osx-font-smoothing: grayscale;
        }

        html {
            scroll-behavior: smooth;
            -webkit-text-size-adjust: 100%;
        }

        /* Navegación Superior Dividida */
        .nav-container {
            position: fixed;
            top: 0;
            left: 0;
            width: 100%;
            z-index: 1000;
            padding: 20px 40px;
            display: flex;
            justify-content: space-between;
            align-items: center;
            pointer-events: none;
        }

        .nav-left, .nav-right {
            display: flex;
            align-items: center;
            gap: 30px;
            pointer-events: all;
        }

        .nav-left {
            background: rgba(21, 21, 21, 0.9);
            -webkit-backdrop-filter: blur(20px);
            backdrop-filter: blur(20px);
            border: 1px solid var(--border);
            border-radius: 50px;
            padding: 12px 30px;
            box-shadow: 0 10px 40px rgba(0, 0, 0, 0.5);
        }

        .nav-item {
            color: var(--text-gray);
            text-decoration: none;
            font-size: 13px;
            font-weight: 500;
            letter-spacing: 1px;
            transition: all 0.3s ease;
            cursor: pointer;
            position: relative;
            text-transform: uppercase;
        }

        .nav-item:hover, .nav-item.active {
            color: var(--text-white);
        }

        .nav-item::after {
            content: '';
            position: absolute;
            bottom: -5px;
            left: 0;
            width: 0;
            height: 2px;
            background: var(--red-accent);
            transition: width 0.3s ease;
        }

        .nav-item:hover::after {
            width: 100%;
        }

        /* Carrito a la derecha */
        .cart-nav {
            background: rgba(21, 21, 21, 0.9);
            -webkit-backdrop-filter: blur(20px);
            backdrop-filter: blur(20px);
            border: 1px solid rgba(213, 34, 44, 0.3);
            border-radius: 50px;
            padding: 12px 25px;
            cursor: pointer;
            display: flex;
            align-items: center;
            gap: 10px;
            transition: all 0.3s;
            box-shadow: 0 10px 40px rgba(0, 0, 0, 0.5);
        }

        .cart-nav:hover {
            background: rgba(213, 34, 44, 0.2);
            transform: scale(1.05);
            border-color: var(--red-accent);
        }

        .cart-icon {
            width: 22px;
            height: 22px;
            fill: var(--red-accent);
        }

        .cart-count {
            background: var(--red-accent);
            color: white;
            font-size: 11px;
            font-weight: 700;
            width: 20px;
            height: 20px;
            border-radius: 50%;
            display: flex;
            align-items: center;
            justify-content: center;
        }

        .cart-total-nav {
            font-size: 14px;
            font-weight: 600;
            color: var(--text-white);
        }

        /* Hero Section - Fondo 65% opacidad */
        .hero {
            height: 100vh;
            width: 100%;
            position: relative;
            display: flex;
            flex-direction: column;
            justify-content: center;
            align-items: center;
            overflow: hidden;
        }

        .hero-bg {
            position: absolute;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            background: linear-gradient(135deg, rgba(10,10,10,0.65) 0%, rgba(20,20,20,0.65) 100%),
                        url('https://images.pexels.com/photos/31302040/pexels-photo-31302040/free-photo-of-elegant-sushi-roll-display-with-chopsticks.jpeg?auto=compress&cs=tinysrgb&dpr=1&w=1920');
            background-size: cover;
            background-position: center;
            -webkit-filter: blur(0px) brightness(0.8);
            filter: blur(0px) brightness(0.8);
            transform: scale(1.1);
            -webkit-animation: breathe 20s ease-in-out infinite;
            animation: breathe 20s ease-in-out infinite;
        }

        @-webkit-keyframes breathe {
            0%, 100% { transform: scale(1.1); }
            50% { transform: scale(1.15); }
        }

        @keyframes breathe {
            0%, 100% { transform: scale(1.1); }
            50% { transform: scale(1.15); }
        }

        .hero-content {
            position: relative;
            z-index: 2;
            text-align: center;
            padding: 20px;
            opacity: 0;
            -webkit-animation: fadeInUp 1.5s ease forwards;
            animation: fadeInUp 1.5s ease forwards;
        }

        @-webkit-keyframes fadeInUp {
            to { opacity: 1; transform: translateY(0); }
            from { opacity: 0; transform: translateY(30px); }
        }

        @keyframes fadeInUp {
            to { opacity: 1; transform: translateY(0); }
            from { opacity: 0; transform: translateY(30px); }
        }

        .logo-container {
            margin-bottom: 30px;
        }

        .logo-img {
            width: 200px;
            height: auto;
            -webkit-filter: drop-shadow(0 10px 30px rgba(213, 34, 44, 0.3));
            filter: drop-shadow(0 10px 30px rgba(213, 34, 44, 0.3));
            transition: transform 0.3s ease;
            display: block;
            margin: 0 auto;
        }

        .logo-img:hover {
            transform: scale(1.05);
        }

        .hero-title {
            font-family: 'Noto Sans JP', sans-serif;
            font-size: 3rem;
            font-weight: 700;
            letter-spacing: 10px;
            margin-bottom: 15px;
            text-shadow: 0 5px 20px rgba(0,0,0,0.8);
            background: linear-gradient(135deg, #fff 0%, var(--text-light) 100%);
            -webkit-background-clip: text;
            background-clip: text;
            -webkit-text-fill-color: transparent;
        }

        .hero-subtitle {
            font-size: 1.2rem;
            color: var(--text-gray);
            font-weight: 300;
            letter-spacing: 3px;
            margin-bottom: 40px;
        }

        .hero-desc {
            max-width: 600px;
            margin: 0 auto 40px;
            color: var(--text-light);
            font-size: 1rem;
            line-height: 1.8;
        }

        .scroll-indicator {
            position: absolute;
            bottom: 40px;
            left: 50%;
            transform: translateX(-50%);
            display: flex;
            flex-direction: column;
            align-items: center;
            gap: 10px;
            opacity: 0.6;
            -webkit-animation: bounce 2s infinite;
            animation: bounce 2s infinite;
            cursor: pointer;
            transition: opacity 0.3s;
        }

        .scroll-indicator:hover { opacity: 1; }

        @-webkit-keyframes bounce {
            0%, 20%, 50%, 80%, 100% { transform: translateX(-50%) translateY(0); }
            40% { transform: translateX(-50%) translateY(-10px); }
            60% { transform: translateX(-50%) translateY(-5px); }
        }

        @keyframes bounce {
            0%, 20%, 50%, 80%, 100% { transform: translateX(-50%) translateY(0); }
            40% { transform: translateX(-50%) translateY(-10px); }
            60% { transform: translateX(-50%) translateY(-5px); }
        }

        .scroll-text {
            font-size: 12px;
            letter-spacing: 2px;
            text-transform: uppercase;
        }

        .scroll-line {
            width: 1px;
            height: 60px;
            background: linear-gradient(to bottom, var(--red-accent), transparent);
        }

        /* About Section */
        .about-section {
            padding: 100px 20px;
            text-align: center;
            opacity: 0.4;
            -webkit-filter: blur(1px);
            filter: blur(1px);
            transition: all 0.5s ease;
        }

        .about-section:hover {
            opacity: 0.8;
            -webkit-filter: blur(0px);
            filter: blur(0px);
        }

        .about-box {
            display: inline-block;
            padding: 15px 40px;
            border: 1px solid var(--border);
            border-radius: 50px;
            background: rgba(255,255,255,0.02);
            font-size: 14px;
            letter-spacing: 3px;
            color: var(--text-gray);
        }

        /* Menu Section */
        .menu-section {
            padding: 80px 20px;
            max-width: 1400px;
            margin: 0 auto;
        }

        .menu-header {
            text-align: center;
            margin-bottom: 60px;
        }

        .menu-title {
            font-family: 'Noto Sans JP', sans-serif;
            font-size: 2.5rem;
            font-weight: 700;
            margin-bottom: 10px;
            letter-spacing: 5px;
        }

        .menu-subtitle {
            color: var(--text-gray);
            font-size: 1rem;
            letter-spacing: 2px;
        }

        .menu-container {
            display: flex;
            gap: 40px;
            align-items: flex-start;
        }

        /* Sidebar - SIEMPRE a la derecha, incluso en móvil */
        .sidebar {
            width: 280px;
            position: -webkit-sticky;
            position: sticky;
            top: 120px;
            flex-shrink: 0;
            order: 2;
        }

        .category-list {
            list-style: none;
            display: flex;
            flex-direction: column;
            gap: 15px;
        }

        .category-item {
            background: rgba(255,255,255,0.03);
            border: 1px solid var(--border);
            border-radius: 20px;
            padding: 25px;
            cursor: pointer;
            transition: all 0.4s ease;
            position: relative;
            overflow: hidden;
            display: flex;
            align-items: center;
            gap: 20px;
        }

        .category-item::before {
            content: '';
            position: absolute;
            left: 0;
            top: 0;
            height: 100%;
            width: 4px;
            background: var(--red-accent);
            transform: scaleY(0);
            transition: transform 0.3s ease;
        }

        .category-item:hover, .category-item.active {
            background: var(--bg-card);
            border-color: var(--red-accent);
            transform: translateX(10px);
        }

        .category-item.active::before {
            transform: scaleY(1);
        }

        .category-icon {
            width: 50px;
            height: 50px;
            flex-shrink: 0;
            transition: transform 0.3s;
        }

        .category-item:hover .category-icon {
            transform: scale(1.1) rotate(5deg);
        }

        .category-info {
            flex: 1;
        }

        .category-name {
            font-size: 16px;
            font-weight: 600;
            letter-spacing: 1px;
            margin-bottom: 5px;
        }

        .category-count {
            font-size: 12px;
            color: var(--text-gray);
        }

        /* Products Grid - A la izquierda */
        .products-grid {
            flex: 1;
            display: grid;
            grid-template-columns: repeat(auto-fill, minmax(300px, 1fr));
            gap: 30px;
            order: 1;
        }

        .product-card {
            background: var(--bg-card);
            border: 1px solid var(--border);
            border-radius: 25px;
            overflow: hidden;
            transition: all 0.4s cubic-bezier(0.4, 0, 0.2, 1);
            position: relative;
            opacity: 0;
            transform: translateY(30px);
        }

        .product-card.visible {
            opacity: 1;
            transform: translateY(0);
            transition: all 0.6s ease;
        }

        .product-card:hover {
            transform: translateY(-10px);
            border-color: rgba(213, 34, 44, 0.3);
            box-shadow: 0 20px 40px rgba(0,0,0,0.4), 0 0 60px rgba(213, 34, 44, 0.1);
        }

        .product-image-container {
            position: relative;
            overflow: hidden;
            height: 200px;
            cursor: pointer;
        }

        .product-image {
            width: 100%;
            height: 100%;
            object-fit: cover;
            transition: transform 0.5s ease;
        }

        .product-card:hover .product-image {
            transform: scale(1.1);
        }

        .product-badge {
            position: absolute;
            top: 15px;
            right: 15px;
            background: var(--red-accent);
            color: white;
            padding: 6px 14px;
            border-radius: 20px;
            font-size: 12px;
            font-weight: 700;
            z-index: 2;
            -webkit-animation: pulse 2s infinite;
            animation: pulse 2s infinite;
        }

        @-webkit-keyframes pulse {
            0%, 100% { transform: scale(1); }
            50% { transform: scale(1.05); }
        }

        @keyframes pulse {
            0%, 100% { transform: scale(1); }
            50% { transform: scale(1.05); }
        }

        .product-info {
            padding: 20px;
        }

        .product-name {
            font-size: 17px;
            font-weight: 600;
            margin-bottom: 6px;
            letter-spacing: 0.5px;
        }

        .product-desc {
            font-size: 13px;
            color: var(--text-gray);
            margin-bottom: 15px;
            line-height: 1.5;
        }

        /* Controles en tarjeta */
        .product-controls {
            display: flex;
            align-items: center;
            gap: 10px;
            margin-bottom: 12px;
        }

        .qty-selector-card {
            display: flex;
            align-items: center;
            gap: 10px;
            background: rgba(255,255,255,0.05);
            border: 1px solid var(--border);
            border-radius: 10px;
            padding: 6px;
        }

        .qty-btn-card {
            width: 28px;
            height: 28px;
            border-radius: 6px;
            border: none;
            background: rgba(255,255,255,0.1);
            color: white;
            font-size: 14px;
            cursor: pointer;
            transition: all 0.3s;
            display: flex;
            align-items: center;
            justify-content: center;
        }

        .qty-btn-card:hover {
            background: var(--red-accent);
        }

        .qty-number-card {
            font-size: 14px;
            font-weight: 600;
            min-width: 25px;
            text-align: center;
        }

        .add-to-cart-btn {
            flex: 1;
            padding: 10px 15px;
            background: var(--red-accent);
            color: white;
            border: none;
            border-radius: 10px;
            font-size: 12px;
            font-weight: 600;
            letter-spacing: 1px;
            cursor: pointer;
            transition: all 0.3s;
            text-transform: uppercase;
        }

        .add-to-cart-btn:hover {
            background: var(--red-hover);
            transform: translateY(-2px);
            box-shadow: 0 5px 15px rgba(213, 34, 44, 0.4);
        }

        .add-to-cart-btn.added {
            background: #25D366;
        }

        .product-price-container {
            display: flex;
            align-items: center;
            justify-content: space-between;
            padding-top: 12px;
            border-top: 1px solid var(--border);
        }

        .product-price {
            font-size: 18px;
            font-weight: 700;
        }

        .product-price .original {
            font-size: 13px;
            color: var(--text-gray);
            text-decoration: line-through;
            margin-right: 8px;
            font-weight: 400;
        }

        /* Modal del Carrito */
        .cart-modal-overlay {
            position: fixed;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            background: rgba(0,0,0,0.95);
            -webkit-backdrop-filter: blur(20px);
            backdrop-filter: blur(20px);
            z-index: 3000;
            display: none;
            justify-content: center;
            align-items: center;
            padding: 20px;
            opacity: 0;
            transition: opacity 0.3s;
        }

        .cart-modal-overlay.active {
            display: flex;
            opacity: 1;
        }

        .cart-modal {
            background: var(--bg-card);
            border: 1px solid var(--border);
            border-radius: 30px;
            max-width: 600px;
            width: 100%;
            max-height: 85vh;
            display: flex;
            flex-direction: column;
            box-shadow: 0 30px 60px rgba(0,0,0,0.5);
            transform: scale(0.9);
            transition: transform 0.3s;
            overflow: hidden;
        }

        .cart-modal-overlay.active .cart-modal {
            transform: scale(1);
        }

        .cart-header {
            padding: 25px 30px;
            border-bottom: 1px solid var(--border);
            display: flex;
            justify-content: space-between;
            align-items: center;
        }

        .cart-title {
            font-size: 1.5rem;
            font-weight: 700;
            display: flex;
            align-items: center;
            gap: 10px;
        }

        .cart-icon-large {
            width: 28px;
            height: 28px;
            fill: var(--red-accent);
        }

        .cart-close {
            width: 40px;
            height: 40px;
            border-radius: 50%;
            border: 1px solid var(--border);
            background: transparent;
            color: white;
            font-size: 20px;
            cursor: pointer;
            transition: all 0.3s;
            display: flex;
            align-items: center;
            justify-content: center;
        }

        .cart-close:hover {
            background: var(--red-accent);
            border-color: var(--red-accent);
            transform: rotate(90deg);
        }

        .cart-items {
            flex: 1;
            overflow-y: auto;
            padding: 20px 30px;
            -webkit-overflow-scrolling: touch;
        }

        .cart-empty {
            text-align: center;
            padding: 60px 20px;
            color: var(--text-gray);
        }

        .cart-empty-icon {
            width: 80px;
            height: 80px;
            margin: 0 auto 20px;
            opacity: 0.3;
        }

        .cart-item {
            display: flex;
            gap: 15px;
            padding: 15px;
            background: rgba(255,255,255,0.03);
            border: 1px solid var(--border);
            border-radius: 15px;
            margin-bottom: 12px;
            transition: all 0.3s;
            position: relative;
            -webkit-animation: slideIn 0.3s ease;
            animation: slideIn 0.3s ease;
        }

        @-webkit-keyframes slideIn {
            from { opacity: 0; transform: translateX(-20px); }
            to { opacity: 1; transform: translateX(0); }
        }

        @keyframes slideIn {
            from { opacity: 0; transform: translateX(-20px); }
            to { opacity: 1; transform: translateX(0); }
        }

        .cart-item:hover {
            background: rgba(255,255,255,0.05);
            border-color: rgba(213, 34, 44, 0.3);
        }

        .cart-item-image {
            width: 70px;
            height: 70px;
            border-radius: 12px;
            object-fit: cover;
        }

        .cart-item-info {
            flex: 1;
            display: flex;
            flex-direction: column;
            justify-content: center;
        }

        .cart-item-name {
            font-size: 15px;
            font-weight: 600;
            margin-bottom: 3px;
        }

        .cart-item-qty {
            font-size: 12px;
            color: var(--text-gray);
        }

        .cart-item-price {
            font-size: 16px;
            font-weight: 700;
            color: var(--red-accent);
            align-self: center;
        }

        .cart-item-remove {
            position: absolute;
            top: 8px;
            right: 8px;
            width: 24px;
            height: 24px;
            border-radius: 50%;
            border: none;
            background: rgba(213, 34, 44, 0.2);
            color: var(--red-accent);
            font-size: 14px;
            cursor: pointer;
            transition: all 0.3s;
            display: flex;
            align-items: center;
            justify-content: center;
        }

        .cart-item-remove:hover {
            background: var(--red-accent);
            color: white;
        }

        .cart-footer {
            padding: 25px 30px;
            border-top: 1px solid var(--border);
            background: linear-gradient(to top, rgba(213, 34, 44, 0.05), transparent);
        }

        .cart-total-section {
            display: flex;
            justify-content: space-between;
            align-items: center;
            margin-bottom: 20px;
            padding: 18px;
            background: rgba(213, 34, 44, 0.1);
            border-radius: 15px;
            border: 1px solid rgba(213, 34, 44, 0.2);
        }

        .cart-total-label {
            font-size: 13px;
            color: var(--text-gray);
            text-transform: uppercase;
            letter-spacing: 2px;
        }

        .cart-total-amount {
            font-size: 1.8rem;
            font-weight: 700;
            color: var(--text-white);
        }

        .cart-checkout-btn {
            width: 100%;
            padding: 18px;
            background: #25D366;
            color: white;
            border: none;
            border-radius: 15px;
            font-size: 15px;
            font-weight: 600;
            letter-spacing: 2px;
            cursor: pointer;
            transition: all 0.3s;
            text-transform: uppercase;
            display: flex;
            align-items: center;
            justify-content: center;
            gap: 10px;
        }

        .cart-checkout-btn:hover {
            background: #128C7E;
            transform: translateY(-2px);
            box-shadow: 0 10px 30px rgba(37, 211, 102, 0.3);
        }

        .cart-checkout-btn:disabled {
            background: var(--text-gray);
            cursor: not-allowed;
            transform: none;
        }

        /* Modal Quick View */
        .product-modal-overlay {
            position: fixed;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            background: rgba(0,0,0,0.9);
            -webkit-backdrop-filter: blur(10px);
            backdrop-filter: blur(10px);
            z-index: 2000;
            display: none;
            justify-content: center;
            align-items: center;
            padding: 20px;
            opacity: 0;
            transition: opacity 0.3s;
        }

        .product-modal-overlay.active {
            display: flex;
            opacity: 1;
        }

        .product-modal {
            background: var(--bg-card);
            border: 1px solid var(--border);
            border-radius: 30px;
            max-width: 500px;
            width: 100%;
            overflow: hidden;
            transform: scale(0.9);
            transition: transform 0.3s;
        }

        .product-modal-overlay.active .product-modal {
            transform: scale(1);
        }

        .product-modal-image {
            width: 100%;
            height: 280px;
            object-fit: cover;
        }

        .product-modal-info {
            padding: 30px;
        }

        .product-modal-name {
            font-size: 1.8rem;
            font-weight: 700;
            margin-bottom: 10px;
        }

        .product-modal-desc {
            color: var(--text-gray);
            margin-bottom: 20px;
            line-height: 1.6;
        }

        .product-modal-price {
            font-size: 2rem;
            font-weight: 700;
            color: var(--red-accent);
            margin-bottom: 25px;
        }

        .product-modal-controls {
            display: flex;
            gap: 15px;
        }

        /* Modal Formulario WhatsApp con Nombre */
        .whatsapp-form-overlay {
            position: fixed;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            background: rgba(0,0,0,0.95);
            -webkit-backdrop-filter: blur(20px);
            backdrop-filter: blur(20px);
            z-index: 4000;
            display: none;
            justify-content: center;
            align-items: center;
            padding: 20px;
            opacity: 0;
            transition: opacity 0.3s;
        }

        .whatsapp-form-overlay.active {
            display: flex;
            opacity: 1;
        }

        .whatsapp-form {
            background: var(--bg-card);
            border: 1px solid var(--border);
            border-radius: 30px;
            max-width: 500px;
            width: 100%;
            padding: 40px;
            transform: scale(0.9);
            transition: transform 0.3s;
        }

        .whatsapp-form-overlay.active .whatsapp-form {
            transform: scale(1);
        }

        .form-header {
            text-align: center;
            margin-bottom: 30px;
        }

        .form-icon {
            font-size: 3rem;
            margin-bottom: 15px;
        }

        .form-title {
            font-size: 1.5rem;
            font-weight: 700;
            margin-bottom: 10px;
        }

        .form-subtitle {
            color: var(--text-gray);
            font-size: 14px;
        }

        .form-group {
            margin-bottom: 20px;
        }

        .form-label {
            display: block;
            font-size: 12px;
            color: var(--text-gray);
            text-transform: uppercase;
            letter-spacing: 1px;
            margin-bottom: 8px;
        }

        .form-input {
            width: 100%;
            padding: 15px;
            background: rgba(255,255,255,0.05);
            border: 1px solid var(--border);
            border-radius: 12px;
            color: white;
            font-size: 16px;
            transition: all 0.3s;
            font-family: 'Ubuntu', sans-serif;
            -webkit-appearance: none;
        }

        .form-input:focus {
            outline: none;
            border-color: var(--red-accent);
            background: rgba(255,255,255,0.08);
        }

        .form-input::placeholder {
            color: rgba(255,255,255,0.3);
        }

        .order-summary {
            background: rgba(255,255,255,0.03);
            border: 1px solid var(--border);
            border-radius: 15px;
            padding: 20px;
            margin-bottom: 25px;
            max-height: 200px;
            overflow-y: auto;
            -webkit-overflow-scrolling: touch;
        }

        .summary-item {
            display: flex;
            justify-content: space-between;
            margin-bottom: 10px;
            font-size: 14px;
        }

        .summary-total {
            border-top: 1px solid var(--border);
            margin-top: 10px;
            padding-top: 10px;
            display: flex;
            justify-content: space-between;
            font-weight: 700;
            font-size: 16px;
            color: var(--red-accent);
        }

        .form-buttons {
            display: flex;
            gap: 15px;
        }

        .btn-secondary {
            flex: 1;
            padding: 15px;
            background: transparent;
            border: 1px solid var(--border);
            color: white;
            border-radius: 12px;
            cursor: pointer;
            transition: all 0.3s;
            font-size: 14px;
        }

        .btn-secondary:hover {
            background: rgba(255,255,255,0.1);
        }

        .btn-primary {
            flex: 2;
            padding: 15px;
            background: #25D366;
            color: white;
            border: none;
            border-radius: 12px;
            cursor: pointer;
            transition: all 0.3s;
            font-size: 16px;
            font-weight: 600;
            display: flex;
            align-items: center;
            justify-content: center;
            gap: 10px;
        }

        .btn-primary:hover {
            background: #128C7E;
            transform: translateY(-2px);
            box-shadow: 0 10px 30px rgba(37, 211, 102, 0.3);
        }

        /* Footer */
        .footer {
            padding: 60px 20px;
            text-align: center;
            border-top: 1px solid var(--border);
            margin-top: 100px;
            background: linear-gradient(to top, rgba(213, 34, 44, 0.05), transparent);
        }

        .footer-logo {
            font-family: 'Noto Sans JP', sans-serif;
            font-size: 2rem;
            font-weight: 700;
            margin-bottom: 20px;
            letter-spacing: 5px;
        }

        .footer-text {
            color: var(--text-gray);
            font-size: 14px;
            margin-bottom: 10px;
        }

        .payment-methods {
            margin-top: 30px;
            padding: 20px;
            background: rgba(255,255,255,0.02);
            border-radius: 20px;
            display: inline-block;
            border: 1px solid var(--border);
        }

        .payment-title {
            font-size: 12px;
            color: var(--text-gray);
            text-transform: uppercase;
            letter-spacing: 2px;
            margin-bottom: 15px;
        }

        .payment-list {
            color: var(--text-light);
            font-size: 14px;
            line-height: 2;
        }

        /* Responsive - Sidebar SIEMPRE derecha */
        @media (max-width: 968px) {
            .nav-container {
                padding: 15px 20px;
            }

            .nav-left {
                padding: 10px 20px;
                gap: 20px;
            }

            .nav-item {
                font-size: 11px;
            }

            .cart-nav {
                padding: 10px 20px;
            }

            .menu-container {
                flex-direction: row;
                gap: 20px;
            }

            .sidebar {
                width: 240px;
                position: -webkit-sticky;
                position: sticky;
                top: 100px;
            }

            .category-item {
                padding: 20px;
            }

            .category-icon {
                width: 40px;
                height: 40px;
            }

            .products-grid {
                grid-template-columns: 1fr;
            }

            .hero-title {
                font-size: 2rem;
            }

            .hero-desc {
                font-size: 0.9rem;
                padding: 0 20px;
            }
        }

        @media (max-width: 640px) {
            .menu-container {
                flex-direction: column-reverse;
            }

            .sidebar {
                width: 100%;
                position: static;
                order: 2;
            }

            .category-list {
                flex-direction: row;
                overflow-x: auto;
                gap: 10px;
                -webkit-overflow-scrolling: touch;
            }

            .category-item {
                min-width: 160px;
                flex-shrink: 0;
            }

            .products-grid {
                order: 1;
            }
        }

        @media (max-width: 480px) {
            .nav-left {
                gap: 15px;
                padding: 8px 15px;
            }

            .cart-total-nav {
                display: none;
            }

            .product-controls {
                flex-direction: column;
            }

            .add-to-cart-btn {
                width: 100%;
            }
        }
    </style>
</head>
<body>

    <!-- Navegación Superior Dividida -->
    <div class="nav-container">
        <div class="nav-left">
            <a class="nav-item active" onclick="scrollToSection('hero')">Inicio</a>
            <a class="nav-item" onclick="scrollToSection('menu')">Menú</a>
            <span class="nav-item" style="opacity: 0.5;">Nosotros</span>
        </div>
        
        <div class="nav-right">
            <div class="cart-nav" onclick="openCart()">
                <svg class="cart-icon" viewBox="0 0 24 24">
                    <path d="M7 18c-1.1 0-1.99.9-1.99 2S5.9 22 7 22s2-.9 2-2-.9-2-2-2zM1 2v2h2l3.6 7.59-1.35 2.45c-.16.28-.25.61-.25.96 0 1.1.9 2 2 2h12v-2H7.42c-.14 0-.25-.11-.25-.25l.03-.12.9-1.63h7.45c.75 0 1.41-.41 1.75-1.03l3.58-6.49c.08-.14.12-.31.12-.48 0-.55-.45-1-1-1H5.21l-.94-2H1zm16 16c-1.1 0-1.99.9-1.99 2s.89 2 1.99 2 2-.9 2-2-.9-2-2-2z"/>
                </svg>
                <span class="cart-count" id="cartCount">0</span>
                <span class="cart-total-nav" id="cartTotalNav">$0</span>
            </div>
        </div>
    </div>

    <!-- Hero Section -->
    <section id="hero" class="hero">
        <div class="hero-bg"></div>
        <div class="hero-content">
            <div class="logo-container">
                <img src="https://raw.githubusercontent.com/liraluis60-sketch/Catalogo-Chino/main/zhuo.png" alt="ZHUO Logo" class="logo-img">
            </div>
            <h1 class="hero-title">ZHUO</h1>
            <p class="hero-subtitle">Fusión Oriental • Sabor Auténtico</p>
            <p class="hero-desc">
                Descubre el equilibrio perfecto entre la tradición asiática y la innovación culinaria. 
                Cada plato es una experiencia única diseñada para despertar tus sentidos.
            </p>
        </div>
        <div class="scroll-indicator" onclick="scrollToSection('about')">
            <span class="scroll-text">Explorar</span>
            <div class="scroll-line"></div>
        </div>
    </section>

    <!-- About Section -->
    <section id="about" class="about-section">
        <div class="about-box">TRADICIÓN & INNOVACIÓN • DESDE 2024</div>
    </section>

    <!-- Menu Section -->
    <section id="menu" class="menu-section">
        <div class="menu-header">
            <h2 class="menu-title">MENÚ</h2>
            <p class="menu-subtitle">Selecciona tu experiencia culinaria</p>
        </div>

        <div class="menu-container">
            <!-- Products Grid - Izquierda -->
            <div class="products-grid" id="productsGrid"></div>

            <!-- Sidebar - Derecha (SIEMPRE) -->
            <aside class="sidebar">
                <ul class="category-list">
                    <li class="category-item active" onclick="filterCategory('sushi', this)">
                        <svg class="category-icon" viewBox="0 0 100 100" fill="none" stroke="currentColor" stroke-width="2">
                            <circle cx="50" cy="50" r="45" stroke="currentColor" stroke-opacity="0.2"/>
                            <path d="M30 50 Q50 30 70 50 Q50 70 30 50" fill="currentColor" fill-opacity="0.1"/>
                            <path d="M35 50 Q50 35 65 50" stroke="currentColor" stroke-linecap="round"/>
                            <circle cx="40" cy="48" r="3" fill="currentColor"/>
                            <circle cx="60" cy="48" r="3" fill="currentColor"/>
                            <path d="M25 55 L20 60 M75 55 L80 60" stroke="currentColor" stroke-linecap="round"/>
                        </svg>
                        <div class="category-info">
                            <div class="category-name">SUSHI</div>
                            <div class="category-count">3 especialidades</div>
                        </div>
                    </li>
                    
                    <li class="category-item" onclick="filterCategory('hamburguesas', this)">
                        <svg class="category-icon" viewBox="0 0 100 100" fill="none" stroke="currentColor" stroke-width="2">
                            <circle cx="50" cy="50" r="45" stroke="currentColor" stroke-opacity="0.2"/>
                            <path d="M25 45 Q50 40 75 45" stroke="currentColor" stroke-linecap="round"/>
                            <path d="M20 50 Q50 45 80 50 L80 55 Q50 60 20 55 Z" fill="currentColor" fill-opacity="0.1"/>
                            <path d="M22 52 L78 52" stroke="currentColor" stroke-dasharray="4 4"/>
                            <path d="M25 58 Q50 63 75 58" stroke="currentColor" stroke-linecap="round"/>
                            <circle cx="35" cy="48" r="2" fill="currentColor"/>
                            <circle cx="50" cy="46" r="2" fill="currentColor"/>
                            <circle cx="65" cy="48" r="2" fill="currentColor"/>
                        </svg>
                        <div class="category-info">
                            <div class="category-name">HAMBURGUESAS</div>
                            <div class="category-count">3 opciones premium</div>
                        </div>
                    </li>
                    
                    <li class="category-item" onclick="filterCategory('bebidas', this)">
                        <svg class="category-icon" viewBox="0 0 100 100" fill="none" stroke="currentColor" stroke-width="2">
                            <circle cx="50" cy="50" r="45" stroke="currentColor" stroke-opacity="0.2"/>
                            <path d="M35 35 L35 70 Q35 75 40 75 L60 75 Q65 75 65 70 L65 35" fill="currentColor" fill-opacity="0.1"/>
                            <path d="M35 35 L65 35" stroke="currentColor"/>
                            <path d="M40 25 Q50 20 60 25" stroke="currentColor" stroke-linecap="round"/>
                            <path d="M50 25 L50 35" stroke="currentColor"/>
                            <path d="M45 45 Q50 50 55 45" stroke="currentColor" stroke-opacity="0.5"/>
                            <path d="M70 40 Q75 45 70 50" stroke="currentColor" stroke-opacity="0.3"/>
                        </svg>
                        <div class="category-info">
                            <div class="category-name">BEBIDAS</div>
                            <div class="category-count">3 refrescantes</div>
                        </div>
                    </li>
                </ul>
            </aside>
        </div>
    </section>

    <!-- Footer -->
    <footer class="footer">
        <div class="footer-logo">ZHUO</div>
        <p class="footer-text">Experiencia culinaria oriental</p>
        <p class="footer-text">© 2024 Todos los derechos reservados</p>
        
        <div class="payment-methods">
            <div class="payment-title">Métodos de Pago Aceptados</div>
            <div class="payment-list">
                Pago Móvil • Zelle • Binance Pay • Transferencia
            </div>
        </div>
    </footer>

    <!-- Modal del Carrito -->
    <div class="cart-modal-overlay" id="cartModal" onclick="closeCart(event)">
        <div class="cart-modal" onclick="event.stopPropagation()">
            <div class="cart-header">
                <h3 class="cart-title">
                    <svg class="cart-icon-large" viewBox="0 0 24 24">
                        <path d="M7 18c-1.1 0-1.99.9-1.99 2S5.9 22 7 22s2-.9 2-2-.9-2-2-2zM1 2v2h2l3.6 7.59-1.35 2.45c-.16.28-.25.61-.25.96 0 1.1.9 2 2 2h12v-2H7.42c-.14 0-.25-.11-.25-.25l.03-.12.9-1.63h7.45c.75 0 1.41-.41 1.75-1.03l3.58-6.49c.08-.14.12-.31.12-.48 0-.55-.45-1-1-1H5.21l-.94-2H1zm16 16c-1.1 0-1.99.9-1.99 2s.89 2 1.99 2 2-.9 2-2-.9-2-2-2z"/>
                    </svg>
                    Tu Pedido
                </h3>
                <button class="cart-close" onclick="closeCart()">×</button>
            </div>
            
            <div class="cart-items" id="cartItems"></div>
            
            <div class="cart-footer">
                <div class="cart-total-section">
                    <span class="cart-total-label">Total a Pagar</span>
                    <span class="cart-total-amount" id="cartTotal">$0</span>
                </div>
                <button class="cart-checkout-btn" onclick="openWhatsAppForm()" id="checkoutBtn">
                    <span>💬</span> Finalizar Pedido
                </button>
            </div>
        </div>
    </div>

    <!-- Modal Quick View -->
    <div class="product-modal-overlay" id="quickViewModal" onclick="closeQuickView(event)">
        <div class="product-modal" onclick="event.stopPropagation()">
            <button class="cart-close" onclick="closeQuickView()" style="position: absolute; top: 15px; right: 15px; z-index: 10;">×</button>
            <img src="" alt="" class="product-modal-image" id="quickViewImage">
            <div class="product-modal-info">
                <h3 class="product-modal-name" id="quickViewName"></h3>
                <p class="product-modal-desc" id="quickViewDesc"></p>
                <div class="product-modal-price" id="quickViewPrice"></div>
                <div class="product-modal-controls">
                    <div class="qty-selector-card">
                        <button class="qty-btn-card" onclick="changeQuickViewQty(-1)">−</button>
                        <span class="qty-number-card" id="quickViewQty">1</span>
                        <button class="qty-btn-card" onclick="changeQuickViewQty(1)">+</button>
                    </div>
                    <button class="add-to-cart-btn" onclick="addToCartFromQuickView()" style="flex: 2;">
                        Agregar al Carrito
                    </button>
                </div>
            </div>
        </div>
    </div>

    <!-- Modal Formulario WhatsApp con Nombre -->
    <div class="whatsapp-form-overlay" id="whatsappForm" onclick="closeWhatsAppForm(event)">
        <div class="whatsapp-form" onclick="event.stopPropagation()">
            <button class="cart-close" onclick="closeWhatsAppForm()" style="position: absolute; top: 20px; right: 20px;">×</button>
            
            <div class="form-header">
                <div class="form-icon">🥢</div>
                <h3 class="form-title">Finalizar Pedido</h3>
                <p class="form-subtitle">Completa tu información para enviar por WhatsApp</p>
            </div>

            <div class="form-group">
                <label class="form-label">Tu Nombre (Opcional)</label>
                <input type="text" class="form-input" id="customerName" placeholder="Ej: Juanito" oninput="updateMessagePreview()">
            </div>

            <div class="form-group">
                <label class="form-label">Resumen del Pedido</label>
                <div class="order-summary" id="orderSummary">
                    <!-- Se genera dinámicamente -->
                </div>
            </div>

            <div class="form-buttons">
                <button class="btn-secondary" onclick="closeWhatsAppForm()">Volver al Carrito</button>
                <button class="btn-primary" onclick="sendToWhatsApp()">
                    <span>📱</span> Enviar a WhatsApp
                </button>
            </div>
        </div>
    </div>

    <script>
        // Datos de productos
        const products = {
            sushi: [
                {
                    id: 1,
                    name: "Dragon Roll Especial",
                    desc: "Langostino tempura, aguacate y salsa de anguila",
                    price: 12,
                    originalPrice: 15,
                    discount: true,
                    image: "https://images.pexels.com/photos/31302040/pexels-photo-31302040/free-photo-of-elegant-sushi-roll-display-with-chopsticks.jpeg?auto=compress&cs=tinysrgb&dpr=1&w=500"
                },
                {
                    id: 2,
                    name: "Sashimi Premium Mix",
                    desc: "Selección de pescados frescos del día",
                    price: 18,
                    originalPrice: null,
                    discount: false,
                    image: "https://www.shutterstock.com/image-photo/sushi-roll-tuna-salmon-avocado-260nw-2272462825.jpg"
                },
                {
                    id: 3,
                    name: "California Especial",
                    desc: "Cangrejo, aguacate y masago con sésamo tostado",
                    price: 10,
                    originalPrice: null,
                    discount: false,
                    image: "https://img.freepik.com/premium-photo/still-life-japanese-healthy-green-tea-small-cups-teapot-dark-background_186277-3911.jpg"
                }
            ],
            hamburguesas: [
                {
                    id: 4,
                    name: "ZHUO Signature Burger",
                    desc: "Doble carne, queso cheddar, cebolla caramelizada",
                    price: 14,
                    originalPrice: null,
                    discount: false,
                    image: "https://www.shutterstock.com/image-photo/homemade-burgers-on-dark-wooden-600nw-2089877563.jpg"
                },
                {
                    id: 5,
                    name: "Oriental Chicken Crispy",
                    desc: "Pollo crispy, salsa teriyaki, pepinillos asiáticos",
                    price: 12,
                    originalPrice: 14,
                    discount: true,
                    image: "https://www.shutterstock.com/image-photo/delicious-gourmet-burger-layers-bacon-600nw-2591611013.jpg"
                },
                {
                    id: 6,
                    name: "Wagyu Truffle",
                    desc: "Carne wagyu, foie gras, trufa negra y huevo",
                    price: 24,
                    originalPrice: null,
                    discount: false,
                    image: "https://www.shutterstock.com/image-photo/tasty-hamburger-flying-ingredients-on-600nw-1349799305.jpg"
                }
            ],
            bebidas: [
                {
                    id: 7,
                    name: "Matcha Latte",
                    desc: "Té verde japonés auténtico con leche",
                    price: 5,
                    originalPrice: null,
                    discount: false,
                    image: "https://img.freepik.com/premium-photo/still-life-japanese-healthy-green-tea-small-cups-teapot-dark-background_186277-3937.jpg?w=360"
                },
                {
                    id: 8,
                    name: "Sake Frutal",
                    desc: "Sake infusionado con frutas tropicales",
                    price: 8,
                    originalPrice: 10,
                    discount: true,
                    image: "https://img.freepik.com/premium-photo/still-life-japanese-healthy-green-tea-small-cups-teapot-dark-background_186277-3911.jpg"
                },
                {
                    id: 9,
                    name: "Yuzu Lemonade",
                    desc: "Limonada con cítricos japoneses y jengibre",
                    price: 6,
                    originalPrice: null,
                    discount: false,
                    image: "https://img.freepik.com/premium-photo/still-life-japanese-healthy-green-tea-small-cups-teapot-dark-background_186277-3937.jpg?w=360"
                }
            ]
        };

        let cart = [];
        let currentCategory = 'sushi';
        let quickViewProduct = null;
        let quickViewQuantity = 1;

        // Inicializar
        document.addEventListener('DOMContentLoaded', () => {
            renderProducts('sushi');
            updateCartUI();
        });

        // Renderizar productos
        function renderProducts(category) {
            const grid = document.getElementById('productsGrid');
            grid.innerHTML = '';
            
            const items = products[category];
            
            items.forEach((product, index) => {
                const card = document.createElement('div');
                card.className = 'product-card';
                card.style.transitionDelay = `${index * 0.1}s`;
                
                const priceHtml = product.discount 
                    ? `<span class="original">$${product.originalPrice}</span> $${product.price}`
                    : `$${product.price}`;
                
                card.innerHTML = `
                    <div class="product-image-container" onclick="openQuickView(${product.id})">
                        ${product.discount ? '<span class="product-badge">-15%</span>' : ''}
                        <img src="${product.image}" alt="${product.name}" class="product-image">
                    </div>
                    <div class="product-info">
                        <h3 class="product-name">${product.name}</h3>
                        <p class="product-desc">${product.desc}</p>
                        
                        <div class="product-controls">
                            <div class="qty-selector-card">
                                <button class="qty-btn-card" onclick="event.stopPropagation(); changeCardQty(${product.id}, -1)">−</button>
                                <span class="qty-number-card" id="qty-${product.id}">1</span>
                                <button class="qty-btn-card" onclick="event.stopPropagation(); changeCardQty(${product.id}, 1)">+</button>
                            </div>
                            <button class="add-to-cart-btn" id="btn-${product.id}" onclick="event.stopPropagation(); addToCartFromCard(${product.id})">
                                AGREGAR
                            </button>
                        </div>
                        
                        <div class="product-price-container">
                            <span class="product-price">${priceHtml}</span>
                        </div>
                    </div>
                `;
                
                grid.appendChild(card);
                
                setTimeout(() => {
                    card.classList.add('visible');
                }, 100);
            });
        }

        // Cambiar cantidad en tarjeta
        function changeCardQty(productId, delta) {
            const qtyEl = document.getElementById(`qty-${productId}`);
            let qty = parseInt(qtyEl.textContent) + delta;
            if (qty < 1) qty = 1;
            if (qty > 20) qty = 20;
            qtyEl.textContent = qty;
        }

        // Agregar al carrito desde tarjeta (SIN abrir carrito)
        function addToCartFromCard(productId) {
            const qty = parseInt(document.getElementById(`qty-${productId}`).textContent);
            const product = findProduct(productId);
            
            if (product) {
                addItemToCart(product, qty);
                
                // Resetear cantidad
                document.getElementById(`qty-${productId}`).textContent = '1';
                
                // Feedback visual en el botón
                const btn = document.getElementById(`btn-${productId}`);
                const originalText = btn.textContent;
                btn.textContent = '✓ AGREGADO';
                btn.classList.add('added');
                
                setTimeout(() => {
                    btn.textContent = originalText;
                    btn.classList.remove('added');
                }, 1500);
            }
        }

        // Encontrar producto
        function findProduct(id) {
            for (let cat in products) {
                const found = products[cat].find(p => p.id === id);
                if (found) return found;
            }
            return null;
        }

        // Agregar item al carrito
        function addItemToCart(product, quantity) {
            const existingItem = cart.find(item => item.id === product.id);
            
            if (existingItem) {
                existingItem.quantity += quantity;
            } else {
                cart.push({
                    id: product.id,
                    name: product.name,
                    price: product.price,
                    image: product.image,
                    quantity: quantity
                });
            }
            
            updateCartUI();
        }

        // Actualizar UI del carrito
        function updateCartUI() {
            const count = cart.reduce((sum, item) => sum + item.quantity, 0);
            const total = cart.reduce((sum, item) => sum + (item.price * item.quantity), 0);
            
            document.getElementById('cartCount').textContent = count;
            document.getElementById('cartTotalNav').textContent = `$${total}`;
            
            // Actualizar items en modal
            const cartItems = document.getElementById('cartItems');
            
            if (cart.length === 0) {
                cartItems.innerHTML = `
                    <div class="cart-empty">
                        <svg class="cart-empty-icon" viewBox="0 0 24 24" fill="currentColor">
                            <path d="M7 18c-1.1 0-1.99.9-1.99 2S5.9 22 7 22s2-.9 2-2-.9-2-2-2zM1 2v2h2l3.6 7.59-1.35 2.45c-.16.28-.25.61-.25.96 0 1.1.9 2 2 2h12v-2H7.42c-.14 0-.25-.11-.25-.25l.03-.12.9-1.63h7.45c.75 0 1.41-.41 1.75-1.03l3.58-6.49c.08-.14.12-.31.12-.48 0-.55-.45-1-1-1H5.21l-.94-2H1zm16 16c-1.1 0-1.99.9-1.99 2s.89 2 1.99 2 2-.9 2-2-.9-2-2-2z"/>
                        </svg>
                        <p>Tu carrito está vacío</p>
                        <p style="font-size: 13px; margin-top: 10px;">¡Agrega algunos productos deliciosos!</p>
                    </div>
                `;
                document.getElementById('checkoutBtn').disabled = true;
            } else {
                cartItems.innerHTML = cart.map(item => `
                    <div class="cart-item">
                        <img src="${item.image}" alt="${item.name}" class="cart-item-image">
                        <div class="cart-item-info">
                            <div class="cart-item-name">${item.name}</div>
                            <div class="cart-item-qty">Cantidad: ${item.quantity}</div>
                        </div>
                        <div class="cart-item-price">$${item.price * item.quantity}</div>
                        <button class="cart-item-remove" onclick="removeFromCart(${item.id})" title="Eliminar">×</button>
                    </div>
                `).join('');
                document.getElementById('checkoutBtn').disabled = false;
            }
            
            document.getElementById('cartTotal').textContent = `$${total}`;
        }

        // Eliminar del carrito
        function removeFromCart(productId) {
            cart = cart.filter(item => item.id !== productId);
            updateCartUI();
        }

        // Abrir/Cerrar carrito
        function openCart() {
            const modal = document.getElementById('cartModal');
            modal.style.display = 'flex';
            setTimeout(() => modal.classList.add('active'), 10);
            document.body.style.overflow = 'hidden';
        }

        function closeCart(event) {
            if (!event || event.target === document.getElementById('cartModal') || event.target.classList.contains('cart-close')) {
                const modal = document.getElementById('cartModal');
                modal.classList.remove('active');
                setTimeout(() => {
                    modal.style.display = 'none';
                    document.body.style.overflow = '';
                }, 300);
            }
        }

        // Quick View Modal
        function openQuickView(productId) {
            quickViewProduct = findProduct(productId);
            quickViewQuantity = 1;
            
            if (!quickViewProduct) return;
            
            document.getElementById('quickViewImage').src = quickViewProduct.image;
            document.getElementById('quickViewName').textContent = quickViewProduct.name;
            document.getElementById('quickViewDesc').textContent = quickViewProduct.desc;
            
            const priceHtml = quickViewProduct.discount 
                ? `$${quickViewProduct.price} <span style="font-size: 1.2rem; color: var(--text-gray); text-decoration: line-through;">$${quickViewProduct.originalPrice}</span>`
                : `$${quickViewProduct.price}`;
            
            document.getElementById('quickViewPrice').innerHTML = priceHtml;
            document.getElementById('quickViewQty').textContent = '1';
            
            const modal = document.getElementById('quickViewModal');
            modal.style.display = 'flex';
            setTimeout(() => modal.classList.add('active'), 10);
            document.body.style.overflow = 'hidden';
        }

        function closeQuickView(event) {
            if (!event || event.target === document.getElementById('quickViewModal') || event.target.classList.contains('cart-close')) {
                const modal = document.getElementById('quickViewModal');
                modal.classList.remove('active');
                setTimeout(() => {
                    modal.style.display = 'none';
                    document.body.style.overflow = '';
                }, 300);
            }
        }

        function changeQuickViewQty(delta) {
            quickViewQuantity += delta;
            if (quickViewQuantity < 1) quickViewQuantity = 1;
            if (quickViewQuantity > 20) quickViewQuantity = 20;
            document.getElementById('quickViewQty').textContent = quickViewQuantity;
        }

        function addToCartFromQuickView() {
            if (quickViewProduct) {
                addItemToCart(quickViewProduct, quickViewQuantity);
                closeQuickView();
            }
        }

        // Formulario WhatsApp con Nombre
        function openWhatsAppForm() {
            closeCart();
            updateOrderSummary();
            const modal = document.getElementById('whatsappForm');
            modal.style.display = 'flex';
            setTimeout(() => modal.classList.add('active'), 10);
            document.body.style.overflow = 'hidden';
        }

        function closeWhatsAppForm(event) {
            if (!event || event.target === document.getElementById('whatsappForm') || event.target.classList.contains('cart-close')) {
                const modal = document.getElementById('whatsappForm');
                modal.classList.remove('active');
                setTimeout(() => {
                    modal.style.display = 'none';
                    document.body.style.overflow = '';
                }, 300);
            }
        }

        function updateOrderSummary() {
            const summary = document.getElementById('orderSummary');
            const total = cart.reduce((sum, item) => sum + (item.price * item.quantity), 0);
            
            let html = '';
            cart.forEach(item => {
                html += `
                    <div class="summary-item">
                        <span>${item.name} x${item.quantity}</span>
                        <span>$${item.price * item.quantity}</span>
                    </div>
                `;
            });
            
            html += `
                <div class="summary-total">
                    <span>TOTAL</span>
                    <span>$${total}</span>
                </div>
            `;
            
            summary.innerHTML = html;
        }

        function sendToWhatsApp() {
            const name = document.getElementById('customerName').value.trim();
            const total = cart.reduce((sum, item) => sum + (item.price * item.quantity), 0);
            
            let message = `🍣 *ZHUO - Nuevo Pedido* 🍣\n\n`;
            
            if (name) {
                message += `¡Hola! Soy *${name}* 👋\n\n`;
            } else {
                message += `¡Hola! 👋\n\n`;
            }
            
            message += `Me gustaría ordenar:\n\n`;
            
            cart.forEach(item => {
                message += `• *${item.name}* x${item.quantity} - $${item.price * item.quantity}\n`;
            });
            
            message += `\n*Total: $${total}*\n\n`;
            message += `¿Podrías enviarme los métodos de pago disponibles? 💳\n\n`;
            message += `¡Gracias! 🥢`;
            
            const encodedMessage = encodeURIComponent(message);
            const phoneNumber = '584125242614';
            const whatsappURL = `https://wa.me/${phoneNumber}?text=${encodedMessage}`;
            
            window.open(whatsappURL, '_blank');
            
            // Cerrar y limpiar
            closeWhatsAppForm();
            cart = [];
            updateCartUI();
            document.getElementById('customerName').value = '';
        }

        // Filtrar categoría
        function filterCategory(category, element) {
            currentCategory = category;
            
            document.querySelectorAll('.category-item').forEach(item => {
                item.classList.remove('active');
            });
            element.classList.add('active');
            
            const grid = document.getElementById('productsGrid');
            grid.style.opacity = '0';
            grid.style.transform = 'translateY(20px)';
            
            setTimeout(() => {
                renderProducts(category);
                grid.style.transition = 'all 0.5s ease';
                grid.style.opacity = '1';
                grid.style.transform = 'translateY(0)';
            }, 300);
        }

        // Scroll a sección
        function scrollToSection(id) {
            const element = document.getElementById(id);
            if (element) {
                element.scrollIntoView({ behavior: 'smooth' });
                
                document.querySelectorAll('.nav-item').forEach(item => {
                    item.classList.remove('active');
                });
                if (event && event.target) {
                    event.target.classList.add('active');
                }
            }
        }

        // Cerrar con ESC
        document.addEventListener('keydown', (e) => {
            if (e.key === 'Escape') {
                closeCart();
                closeQuickView();
                closeWhatsAppForm();
            }
        });
    </script>
</body>
</html>
