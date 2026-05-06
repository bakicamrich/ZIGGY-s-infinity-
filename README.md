// Mobile Menu Toggle
const hamburger = document.querySelector('.hamburger');
const navMenu = document.querySelector('.nav-menu');
const navLinks = document.querySelectorAll('.nav-link');

// Toggle mobile menu
hamburger.addEventListener('click', () => {
    hamburger.classList.toggle('active');
    navMenu.classList.toggle('active');
});

// Close mobile menu when a link is clicked
navLinks.forEach(link => {
    link.addEventListener('click', () => {
        hamburger.classList.remove('active');
        navMenu.classList.remove('active');
    });
});

// Smooth scroll and active link highlight
window.addEventListener('scroll', () => {
    let current = '';
    const sections = document.querySelectorAll('.section, #home');
    
    sections.forEach(section => {
        const sectionTop = section.offsetTop;
        const sectionHeight = section.clientHeight;
        
        if (scrollY >= sectionTop - 200) {
            current = section.getAttribute('id');
        }
    });

    navLinks.forEach(link => {
        link.classList.remove('active');
        if (link.getAttribute('href').slice(1) === current) {
            link.classList.add('active');
        }
    });
});

// Scroll animations for cards
const observerOptions = {
    threshold: 0.1,
    rootMargin: '0px 0px -100px 0px'
};

const observer = new IntersectionObserver((entries) => {
    entries.forEach(entry => {
        if (entry.isIntersecting) {
            entry.target.style.animation = 'fadeInUp 0.6s ease forwards';
            observer.unobserve(entry.target);
        }
    });
}, observerOptions);

// Observe service cards
document.querySelectorAll('.service-card').forEach(card => {
    observer.observe(card);
});

// Contact Form Handling
const contactForm = document.getElementById('contactForm');

if (contactForm) {
    contactForm.addEventListener('submit', (e) => {
        e.preventDefault();
        
        const name = contactForm.querySelector('input[type="text"]').value;
        const email = contactForm.querySelector('input[type="email"]').value;
        const phone = contactForm.querySelector('input[type="tel"]').value;
        const service = contactForm.querySelector('select').value;
        const message = contactForm.querySelector('textarea').value;
        
        // Create WhatsApp message
        const whatsappMessage = encodeURIComponent(
            `Hello ZIGGY's Infinity!\n\n` +
            `I would like to inquire about: ${service}\n\n` +
            `Name: ${name}\n` +
            `Email: ${email}\n` +
            `Phone: ${phone}\n\n` +
            `Message: ${message}`
        );
        
        // Open WhatsApp with pre-filled message
        window.open(`https://wa.me/27790603727?text=${whatsappMessage}`, '_blank');
        
        // Reset form
        contactForm.reset();
        
        // Show confirmation message
        alert('Thank you for your message! You will be redirected to WhatsApp to complete your inquiry.');
    });
}

// Counter animation for statistics
const counters = document.querySelectorAll('.stat h3');
const speed = 200; // Lower number = faster

counters.forEach(counter => {
    const updateCount = () => {
        const target = parseInt(counter.getAttribute('data-target')) || parseInt(counter.textContent.replace(/[^0-9]/g, ''));
        const increment = target / speed;
        let current = 0;

        const updateCounter = () => {
            current += increment;
            if (current < target) {
                counter.textContent = Math.ceil(current) + '+';
                setTimeout(updateCounter, 10);
            } else {
                counter.textContent = target + '+';
            }
        };

        // Trigger animation when element is in view
        const observerStats = new IntersectionObserver((entries) => {
            entries.forEach(entry => {
                if (entry.isIntersecting && !counter.classList.contains('counted')) {
                    counter.classList.add('counted');
                    updateCounter();
                }
            });
        }, { threshold: 0.5 });

        observerStats.observe(counter);
    };

    updateCount();
});

// Fade-in animation on page load
window.addEventListener('load', () => {
    document.body.style.opacity = '1';
});

// Prevent page from jumping when clicking links
document.querySelectorAll('a[href^="#"]').forEach(anchor => {
    anchor.addEventListener('click', function (e) {
        if (this.getAttribute('href') !== '#') {
            e.preventDefault();
            const target = document.querySelector(this.getAttribute('href'));
            if (target) {
                target.scrollIntoView({
                    behavior: 'smooth',
                    block: 'start'
                });
            }
        }
    });
});

// Add loading class to images
const images = document.querySelectorAll('img');
images.forEach(img => {
    img.addEventListener('load', () => {
        img.classList.add('loaded');
    });
});

console.log('ZIGGY\'s Infinity Website - Loaded Successfully!');
