---
layout: page
title: Certificates
icon: fas fa-certificate
order: 5
---

## 🎓 Professional Certifications

<div class="thm-achievement-wrapper">
    <div class="thm-card">
        <div class="cert-image-container">
            <a href="https://tryhackme-certificates.s3-eu-west-1.amazonaws.com/THM-U9CJUSF2IG.pdf" target="_blank" rel="noopener noreferrer" title="View Verified PDF Certificate">
                <img src="/assets/img/cert/ThmCer.jpg" alt="TryHackMe 'Love at First Breach' CTF Certificate" class="cert-preview-img">
                <div class="img-overlay">
                    <span class="icon-view">PDF</span>
                </div>
            </a>
        </div>
        
        <div class="cert-details">
            <div class="thm-logo-area">
                <img src="https://tryhackme.com/static/img/thm-logo.png" alt="TryHackMe Logo" class="thm-inline-logo">
            </div>
            
            <h3 class="event-name">Love at First Breach <span class="ctf-tag">CTF</span></h3>
            <p class="issuer">Issued by: <strong>TryHackMe</strong></p>
            
            <div class="achievement-metrics">
                <div class="metric-item">
                    <span class="metric-icon">🏆</span>
                    <span class="metric-text">Global Rank: <strong>Top 5%</strong></span>
                </div>
                <div class="metric-item">
                    <span class="metric-icon">🔥</span>
                    <span class="metric-text">Issue Date: <strong>Feb 25, 2026</strong></span>
                </div>
            </div>
            
            <p class="description">Demonstrated practical proficiency in <strong>offensive security</strong> and <strong>reverse engineering</strong> challenges.</p>
            
            <div class="action-footer">
                <a href="https://tryhackme-certificates.s3-eu-west-1.amazonaws.com/THM-U9CJUSF2IG.pdf" target="_blank" rel="noopener noreferrer" class="btn-primary">Verify Credential</a>
            </div>
        </div>
    </div>
</div>

<style>
/* CSS styles for a professional dark cyber aesthetic */
.thm-achievement-wrapper {
    width: 100%;
    max-width: 900px; /* Adjust to match your site content width */
    margin: 40px auto;
    font-family: 'Segoe UI', Roboto, 'Helvetica Neue', Arial, sans-serif;
    color: #e0e6ed;
}

.thm-card {
    display: flex;
    background: #11111b; /* Rich dark background */
    border: 1px solid #2d3748;
    border-radius: 16px;
    overflow: hidden;
    box-shadow: 0 10px 30px rgba(0, 0, 0, 0.4);
    transition: transform 0.3s ease, box-shadow 0.3s ease;
}

.thm-card:hover {
    transform: translateY(-5px);
    box-shadow: 0 15px 40px rgba(0, 180, 216, 0.3); /* Electric blue glow on hover */
    border-color: #00b4d8;
}

/* Image Side */
.cert-image-container {
    flex: 2; /* Adjust width relative to details */
    position: relative;
    overflow: hidden;
    cursor: pointer;
}

.cert-preview-img {
    width: 100%;
    height: 100%;
    object-fit: cover;
    transition: filter 0.3s ease;
}

.img-overlay {
    position: absolute;
    top: 0; left: 0;
    width: 100%; height: 100%;
    background: rgba(0, 0, 0, 0.6);
    display: flex;
    justify-content: center;
    align-items: center;
    opacity: 0;
    transition: opacity 0.3s ease;
}

.img-overlay .icon-view {
    font-size: 1.2rem;
    color: #fff;
    border: 2px solid #fff;
    padding: 8px 16px;
    border-radius: 6px;
    font-weight: 700;
}

.cert-image-container:hover .cert-preview-img {
    filter: blur(2px);
}

.cert-image-container:hover .img-overlay {
    opacity: 1;
}

/* Details Side */
.cert-details {
    flex: 3; /* Adjust width relative to image */
    padding: 30px;
    display: flex;
    flex-direction: column;
}

.thm-logo-area {
    margin-bottom: 20px;
}

.thm-inline-logo {
    height: 35px; /* Size TryHackMe logo elegantly */
    width: auto;
}

.event-name {
    margin: 0 0 8px 0;
    font-size: 1.6rem;
    font-weight: 800;
    color: #fff;
}

.ctf-tag {
    font-size: 0.9rem;
    background: #00b4d8;
    color: #fff;
    padding: 4px 8px;
    border-radius: 4px;
    margin-left: 8px;
    vertical-align: middle;
}

.issuer {
    margin: 0 0 15px 0;
    font-size: 1rem;
    color: #b1b1b1;
}

.issuer strong { color: #fff; }

.achievement-metrics {
    display: flex;
    gap: 20px;
    margin-bottom: 15px;
    flex-wrap: wrap;
}

.metric-item {
    display: flex;
    align-items: center;
    background: #1e1e2d;
    padding: 8px 12px;
    border-radius: 8px;
    font-size: 0.9rem;
}

.metric-icon { margin-right: 8px; font-size: 1.1rem; }
.metric-text { color: #b1b1b1; }
.metric-text strong { color: #fff; }

.description {
    margin: 0 0 25px 0;
    font-size: 0.95rem;
    line-height: 1.6;
    color: #b1b1b1;
}

.description strong { color: #fff; }

.action-footer {
    margin-top: auto;
}

.btn-primary {
    display: inline-block;
    background: #00b4d8;
    color: #fff;
    text-decoration: none;
    font-size: 0.95rem;
    font-weight: 700;
    padding: 12px 24px;
    border-radius: 8px;
    transition: background 0.2s ease, transform 0.2s ease;
}

.btn-primary:hover {
    background: #008eb6;
    transform: translateY(-2px);
}

/* Responsive design for tablets and mobile */
@media (max-width: 768px) {
    .thm-card { flex-direction: column; }
    .cert-image-container { height: 250px; flex: unset; }
    .cert-details { flex: unset; padding: 25px; text-align: center; align-items: center; }
    .achievement-metrics { justify-content: center; }
}
</style>
