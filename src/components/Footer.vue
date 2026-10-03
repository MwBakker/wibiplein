<template>
    <footer class="site-footer">
        <div class="footer-content">
            <div id="admin-info">
                <p class="copyright">&copy; 2026 Wibi Plein. Alle rechten voorbehouden.</p>
                <p>Adres: Pleinweg 1, Drenthe</p>
                <p>KvK: 12345678</p>
                <p>BTW: NL123456789B01</p>
            </div>
            
            <div id="map-container">
                <iframe 
                    title="Google Maps Locatie Wibi Plein"
                    src="https://www.google.com/maps/embed?pb=!1m18!1m12!1m3!1d2436.72828628373!2d6.8!3d52.6!2m3!1f0!2f0!3f0!3m2!1i1024!2i768!4f13.1!3m3!1m2!1s0x0%3A0x0!2zNTLCsDM2JzAwLjAiTiA2wrA0OCcwMC4wIkU!5e0!3m2!1snl!2snl!4v1600000000000!5m2!1snl!2snl" 
                    width="100%" 
                    height="100%" 
                    style="border:0;" 
                    loading="lazy" 
                    referrerpolicy="no-referrer-when-downgrade">
                </iframe>
            </div>
        </div>
    </footer>
</template>

<style scoped>
.site-footer {
    width: 100vw;
    position: relative;
    background-color: rgba(250, 249, 246, 0.9);
    box-shadow: 0 -4px 20px rgba(0, 0, 0, 0.08);
    z-index: 1000;
    display: flex;
    justify-content: center;
    align-items: center;
    overflow-x: hidden;
}

.site-footer::before {
    content: '';
    position: absolute;
    inset: 0;
    backdrop-filter: blur(20px);
    -webkit-backdrop-filter: blur(20px);
    z-index: -1;
    -webkit-mask-image: linear-gradient(to top, rgba(0, 0, 0, 1) 0%, rgba(0, 0, 0, 0.8) 40%, rgba(0, 0, 0, 0) 100%);
    mask-image: linear-gradient(to top, rgba(0, 0, 0, 1) 0%, rgba(0, 0, 0, 0.8) 40%, rgba(0, 0, 0, 0) 100%);
}

.footer-content {
    width: 100%;
    max-width: 1280px;
    display: flex;
    justify-content: space-between;
    align-items: center;
    color: #1B3b5b;
    font-weight: 600;
    gap: 32px;
}

#admin-info {
    display: flex;
    flex-direction: column;
    gap: 6px;
    font-size: 0.95rem;
    flex-shrink: 0;
}

.copyright {
    font-size: 1rem;
    font-weight: 700;
    margin-bottom: 4px;
}

#map-container {
    width: 480px;
    height: 320px;
    border-radius: 12px 0 0 12px;
    overflow: hidden;
    border: 2px solid #1B3b5b;
    border-right: none;
    box-shadow: 0 4px 12px rgba(27, 59, 91, 0.1);
    margin-right: calc(-50vw + 50%);
}

@media (max-width: 1024px) {
    .site-footer {
        padding: 30px 20px;
    }

    .footer-content {
        flex-direction: column;
        align-items: flex-start;
    }

    #map-container {
        height: 220px;
        margin-right: 0;
        border-right: 2px solid #1B3b5b;
        border-radius: 12px;
    }
}
</style>