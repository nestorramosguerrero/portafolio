[script.js](https://github.com/user-attachments/files/23067635/script.js)
// ===== CONFIGURACIÓN INICIAL =====
document.addEventListener('DOMContentLoaded', function() {
    console.log('🚀 El Ingeniero i-Fix - Página cargada correctamente');
    
    // Inicializar todas las funcionalidades
    initNavigation();
    initScrollEffects();
    initFormHandling();
    initAnimations();
    initParticleEffects();
    initProfessionalEffects();
});

// ===== NAVEGACIÓN RESPONSIVE =====
function initNavigation() {
    const hamburger = document.querySelector('.hamburger');
    const navMenu = document.querySelector('.nav-menu');
    const navLinks = document.querySelectorAll('.nav-link');
    
    // Toggle del menú hamburguesa
    hamburger.addEventListener('click', function() {
        hamburger.classList.toggle('active');
        navMenu.classList.toggle('active');
        
        // Animación de las barras del hamburguesa
        const bars = hamburger.querySelectorAll('.bar');
        bars.forEach((bar, index) => {
            if (hamburger.classList.contains('active')) {
                if (index === 0) bar.style.transform = 'rotate(-45deg) translate(-5px, 6px)';
                if (index === 1) bar.style.opacity = '0';
                if (index === 2) bar.style.transform = 'rotate(45deg) translate(-5px, -6px)';
            } else {
                bar.style.transform = 'none';
                bar.style.opacity = '1';
            }
        });
    });
    
    // Cerrar menú al hacer clic en un enlace
    navLinks.forEach(link => {
        link.addEventListener('click', function() {
            hamburger.classList.remove('active');
            navMenu.classList.remove('active');
            
            // Resetear animación de barras
            const bars = hamburger.querySelectorAll('.bar');
            bars.forEach(bar => {
                bar.style.transform = 'none';
                bar.style.opacity = '1';
            });
        });
    });
    
    // Smooth scroll para enlaces de navegación
    navLinks.forEach(link => {
        link.addEventListener('click', function(e) {
            e.preventDefault();
            const targetId = this.getAttribute('href');
            const targetSection = document.querySelector(targetId);
            
            if (targetSection) {
                const offsetTop = targetSection.offsetTop - 80; // Ajustar por navbar fijo
                window.scrollTo({
                    top: offsetTop,
                    behavior: 'smooth'
                });
            }
        });
    });
}

// ===== EFECTOS DE SCROLL =====
function initScrollEffects() {
    const navbar = document.querySelector('.navbar');
    const sections = document.querySelectorAll('section');
    const navLinks = document.querySelectorAll('.nav-link');
    
    // Efecto de transparencia en navbar al hacer scroll
    window.addEventListener('scroll', function() {
        const scrollY = window.scrollY;
        
        if (scrollY > 100) {
            navbar.style.background = 'rgba(10, 12, 22, 0.98)';
            navbar.style.boxShadow = '0 2px 20px rgba(56, 189, 248, 0.3)';
        } else {
            navbar.style.background = 'rgba(10, 12, 22, 0.95)';
            navbar.style.boxShadow = 'none';
        }
        
        // Actualizar enlace activo según la sección visible
        updateActiveNavLink();
        
        // Efectos de parallax en elementos
        applyParallaxEffects(scrollY);
    });
    
    // Función para actualizar el enlace activo
    function updateActiveNavLink() {
        let current = '';
        sections.forEach(section => {
            const sectionTop = section.offsetTop - 100;
            const sectionHeight = section.clientHeight;
            
            if (window.scrollY >= sectionTop && window.scrollY < sectionTop + sectionHeight) {
                current = section.getAttribute('id');
            }
        });
        
        navLinks.forEach(link => {
            link.classList.remove('active');
            if (link.getAttribute('href') === `#${current}`) {
                link.classList.add('active');
            }
        });
    }
    
    // Efectos de parallax
    function applyParallaxEffects(scrollY) {
        const heroBackground = document.querySelector('.hero-background');
        const gridOverlay = document.querySelector('.grid-overlay');
        
        if (heroBackground && gridOverlay) {
            const speed = scrollY * 0.5;
            gridOverlay.style.transform = `translateY(${speed}px)`;
        }
    }
}

// ===== MANEJO DEL FORMULARIO =====
function initFormHandling() {
    const contactForm = document.getElementById('contactForm');
    
    if (contactForm) {
        contactForm.addEventListener('submit', function(e) {
            e.preventDefault();
            
            // Obtener datos del formulario
            const formData = new FormData(this);
            const nombre = formData.get('nombre');
            const email = formData.get('email');
            const mensaje = formData.get('mensaje');
            
            // Validar campos
            if (validateForm(nombre, email, mensaje)) {
                // Simular envío del formulario
                simulateFormSubmission();
                
                // Mostrar mensaje en consola como se solicita
                console.log('Formulario enviado correctamente');
                
                // Mostrar notificación visual
                showNotification('¡Mensaje enviado correctamente!', 'success');
                
                // Limpiar formulario
                this.reset();
            } else {
                showNotification('Por favor, completa todos los campos correctamente', 'error');
            }
        });
        
        // Efectos visuales en campos del formulario
        const formInputs = contactForm.querySelectorAll('input, textarea');
        formInputs.forEach(input => {
            input.addEventListener('focus', function() {
                this.parentElement.classList.add('focused');
            });
            
            input.addEventListener('blur', function() {
                if (!this.value) {
                    this.parentElement.classList.remove('focused');
                }
            });
        });
    }
}

// ===== VALIDACIÓN DEL FORMULARIO =====
function validateForm(nombre, email, mensaje) {
    // Validar que todos los campos estén completos
    if (!nombre || !email || !mensaje) {
        return false;
    }
    
    // Validar formato de email
    const emailRegex = /^[^\s@]+@[^\s@]+\.[^\s@]+$/;
    if (!emailRegex.test(email)) {
        return false;
    }
    
    // Validar longitud mínima del mensaje
    if (mensaje.length < 10) {
        return false;
    }
    
    return true;
}

// ===== SIMULACIÓN DE ENVÍO =====
function simulateFormSubmission() {
    const submitButton = document.querySelector('#contactForm button[type="submit"]');
    const originalText = submitButton.innerHTML;
    
    // Cambiar estado del botón
    submitButton.innerHTML = '<i class="fas fa-spinner fa-spin"></i> Enviando...';
    submitButton.disabled = true;
    
    // Simular delay de envío
    setTimeout(() => {
        submitButton.innerHTML = originalText;
        submitButton.disabled = false;
    }, 2000);
}

// ===== NOTIFICACIONES =====
function showNotification(message, type = 'info') {
    // Crear elemento de notificación
    const notification = document.createElement('div');
    notification.className = `notification notification-${type}`;
    notification.innerHTML = `
        <div class="notification-content">
            <i class="fas fa-${type === 'success' ? 'check-circle' : type === 'error' ? 'exclamation-circle' : 'info-circle'}"></i>
            <span>${message}</span>
        </div>
    `;
    
    // Agregar estilos dinámicos
    notification.style.cssText = `
        position: fixed;
        top: 100px;
        right: 20px;
        background: ${type === 'success' ? 'rgba(16, 185, 129, 0.9)' : type === 'error' ? 'rgba(239, 68, 68, 0.9)' : 'rgba(56, 189, 248, 0.9)'};
        color: white;
        padding: 1rem 1.5rem;
        border-radius: 8px;
        box-shadow: 0 10px 30px rgba(0, 0, 0, 0.3);
        z-index: 10000;
        transform: translateX(400px);
        transition: transform 0.3s ease;
        backdrop-filter: blur(10px);
        border: 1px solid rgba(255, 255, 255, 0.2);
    `;
    
    // Agregar al DOM
    document.body.appendChild(notification);
    
    // Animar entrada
    setTimeout(() => {
        notification.style.transform = 'translateX(0)';
    }, 100);
    
    // Remover después de 4 segundos
    setTimeout(() => {
        notification.style.transform = 'translateX(400px)';
        setTimeout(() => {
            if (notification.parentNode) {
                notification.parentNode.removeChild(notification);
            }
        }, 300);
    }, 4000);
}

// ===== ANIMACIONES Y EFECTOS =====
function initAnimations() {
    // Intersection Observer para animaciones al hacer scroll
    const observerOptions = {
        threshold: 0.1,
        rootMargin: '0px 0px -50px 0px'
    };
    
    const observer = new IntersectionObserver((entries) => {
        entries.forEach(entry => {
            if (entry.isIntersecting) {
                entry.target.classList.add('animate-in');
                
                // Efectos especiales para diferentes elementos
                if (entry.target.classList.contains('servicio-card')) {
                    animateServiceCard(entry.target);
                } else if (entry.target.classList.contains('proyecto-card')) {
                    animateProjectCard(entry.target);
                }
            }
        });
    }, observerOptions);
    
    // Observar elementos animables
    const animatedElements = document.querySelectorAll('.servicio-card, .proyecto-card, .info-card');
    animatedElements.forEach(el => {
        observer.observe(el);
    });
    
    // Efectos hover mejorados
    initHoverEffects();
}

// ===== ANIMACIONES DE TARJETAS =====
function animateServiceCard(card) {
    const icon = card.querySelector('.servicio-icon');
    const delay = Array.from(card.parentNode.children).indexOf(card) * 100;
    
    setTimeout(() => {
        card.style.transform = 'translateY(0)';
        card.style.opacity = '1';
        
        if (icon) {
            icon.style.transform = 'scale(1)';
            icon.style.boxShadow = '0 0 30px rgba(56, 189, 248, 0.6)';
        }
    }, delay);
}

function animateProjectCard(card) {
    const image = card.querySelector('.proyecto-image img');
    const delay = Array.from(card.parentNode.children).indexOf(card) * 150;
    
    setTimeout(() => {
        card.style.transform = 'translateY(0)';
        card.style.opacity = '1';
        
        if (image) {
            image.style.transform = 'scale(1)';
        }
    }, delay);
}

// ===== EFECTOS HOVER MEJORADOS =====
function initHoverEffects() {
    // Efectos en botones
    const buttons = document.querySelectorAll('.btn');
    buttons.forEach(btn => {
        btn.addEventListener('mouseenter', function() {
            this.style.transform = 'translateY(-3px) scale(1.05)';
        });
        
        btn.addEventListener('mouseleave', function() {
            this.style.transform = 'translateY(0) scale(1)';
        });
    });
    
    // Efectos en tarjetas de servicios
    const serviceCards = document.querySelectorAll('.servicio-card');
    serviceCards.forEach(card => {
        card.addEventListener('mouseenter', function() {
            const icon = this.querySelector('.servicio-icon');
            if (icon) {
                icon.style.transform = 'scale(1.1) rotate(5deg)';
                icon.style.boxShadow = '0 0 40px rgba(56, 189, 248, 0.8)';
            }
        });
        
        card.addEventListener('mouseleave', function() {
            const icon = this.querySelector('.servicio-icon');
            if (icon) {
                icon.style.transform = 'scale(1) rotate(0deg)';
                icon.style.boxShadow = '0 0 20px rgba(56, 189, 248, 0.5)';
            }
        });
    });
    
    // Efectos en enlaces sociales
    const socialLinks = document.querySelectorAll('.social-link');
    socialLinks.forEach(link => {
        link.addEventListener('mouseenter', function() {
            this.style.transform = 'translateX(10px) scale(1.05)';
        });
        
        link.addEventListener('mouseleave', function() {
            this.style.transform = 'translateX(0) scale(1)';
        });
    });
}

// ===== EFECTOS DE PARTÍCULAS =====
function initParticleEffects() {
    // Crear partículas flotantes dinámicas
    createFloatingParticles();
    
    // Efecto de cursor con partículas
    initCursorParticles();
}

function createFloatingParticles() {
    const particleContainer = document.createElement('div');
    particleContainer.className = 'particle-container';
    particleContainer.style.cssText = `
        position: fixed;
        top: 0;
        left: 0;
        width: 100%;
        height: 100%;
        pointer-events: none;
        z-index: -1;
    `;
    
    document.body.appendChild(particleContainer);
    
    // Crear partículas periódicamente
    setInterval(() => {
        createParticle(particleContainer);
    }, 2000);
}

function createParticle(container) {
    const particle = document.createElement('div');
    const size = Math.random() * 2 + 1;
    const startX = Math.random() * window.innerWidth;
    const colors = ['#3b82f6', '#1d4ed8', '#1e40af'];
    const color = colors[Math.floor(Math.random() * colors.length)];
    
    particle.style.cssText = `
        position: absolute;
        width: ${size}px;
        height: ${size}px;
        background: ${color};
        border-radius: 50%;
        left: ${startX}px;
        top: 100vh;
        opacity: 0.6;
        animation: float-up 12s linear forwards;
    `;
    
    container.appendChild(particle);
    
    // Remover partícula después de la animación
    setTimeout(() => {
        if (particle.parentNode) {
            particle.parentNode.removeChild(particle);
        }
    }, 12000);
}

function initCursorParticles() {
    let mouseX = 0;
    let mouseY = 0;
    
    document.addEventListener('mousemove', function(e) {
        mouseX = e.clientX;
        mouseY = e.clientY;
        
        // Crear partícula ocasional en el cursor
        if (Math.random() < 0.1) {
            createCursorParticle(mouseX, mouseY);
        }
    });
}

function createCursorParticle(x, y) {
    const particle = document.createElement('div');
    particle.style.cssText = `
        position: fixed;
        width: 2px;
        height: 2px;
        background: #3b82f6;
        border-radius: 50%;
        left: ${x}px;
        top: ${y}px;
        pointer-events: none;
        z-index: 1000;
        opacity: 0.7;
        animation: cursor-particle 1.5s ease-out forwards;
    `;
    
    document.body.appendChild(particle);
    
    setTimeout(() => {
        if (particle.parentNode) {
            particle.parentNode.removeChild(particle);
        }
    }, 1500);
}

// ===== EFECTOS DE TEXTO PROFESIONAL =====
function initProfessionalEffects() {
    const titleElements = document.querySelectorAll('.hero-title');
    
    titleElements.forEach(element => {
        // Efecto sutil de aparición
        element.style.opacity = '0';
        element.style.transform = 'translateY(20px)';
        
        setTimeout(() => {
            element.style.transition = 'all 0.8s ease-out';
            element.style.opacity = '1';
            element.style.transform = 'translateY(0)';
        }, 500);
    });
}

// ===== UTILIDADES =====
function debounce(func, wait) {
    let timeout;
    return function executedFunction(...args) {
        const later = () => {
            clearTimeout(timeout);
            func(...args);
        };
        clearTimeout(timeout);
        timeout = setTimeout(later, wait);
    };
}

function throttle(func, limit) {
    let inThrottle;
    return function() {
        const args = arguments;
        const context = this;
        if (!inThrottle) {
            func.apply(context, args);
            inThrottle = true;
            setTimeout(() => inThrottle = false, limit);
        }
    };
}

// ===== CSS DINÁMICO PARA ANIMACIONES =====
const dynamicStyles = `
    @keyframes float-up {
        0% {
            transform: translateY(0) rotate(0deg);
            opacity: 1;
        }
        100% {
            transform: translateY(-100vh) rotate(360deg);
            opacity: 0;
        }
    }
    
    @keyframes cursor-particle {
        0% {
            transform: scale(1);
            opacity: 1;
        }
        100% {
            transform: scale(0);
            opacity: 0;
        }
    }
    
    .animate-in {
        animation: slideInUp 0.6s ease-out forwards;
    }
    
    @keyframes slideInUp {
        0% {
            transform: translateY(50px);
            opacity: 0;
        }
        100% {
            transform: translateY(0);
            opacity: 1;
        }
    }
    
    .servicio-card,
    .proyecto-card,
    .info-card {
        transform: translateY(50px);
        opacity: 0;
        transition: all 0.6s ease-out;
    }
    
    .servicio-icon {
        transition: all 0.3s ease;
    }
    
    .form-group.focused label {
        color: #38bdf8;
        transform: translateY(-5px);
    }
    
    .nav-link.active {
        color: #38bdf8;
        text-shadow: 0 0 20px rgba(56, 189, 248, 0.5);
    }
`;

// Agregar estilos dinámicos al documento
const styleSheet = document.createElement('style');
styleSheet.textContent = dynamicStyles;
document.head.appendChild(styleSheet);

// ===== INICIALIZACIÓN FINAL =====
// Inicializar efectos profesionales después de un delay
setTimeout(() => {
    initProfessionalEffects();
}, 1000);

// Log de inicialización completa
console.log('✨ Todos los efectos y funcionalidades han sido inicializados correctamente');
console.log('🎯 El Ingeniero i-Fix está listo para recibir visitantes');
