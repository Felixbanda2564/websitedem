function showMessage(){
    alert('Welcome to Velvet Touch - Beauty at your fingertips, Taste every Bite');
}

const WA_PHONES = ['260883199259', '260880839099'];

function openWhatsAppChats(message){
    WA_PHONES.forEach((phone, index) => {
        const url = `https://wa.me/${phone}?text=${encodeURIComponent(message)}`;
        if(index === 0){
            window.open(url, '_blank');
        } else {
            setTimeout(() => window.open(url, '_blank'), 300);
        }
    });
}

function orderWhatsApp(){
    const qtyInput = document.getElementById('pieQty');
    const notesInput = document.getElementById('pieNotes');
    const orderSummary = document.getElementById('order');

    if(!qtyInput) return;

    const qty = Math.max(1, parseInt(qtyInput.value || '1', 10));
    const notes = (notesInput && notesInput.value) ? notesInput.value.trim() : '';

    const summaryText = `Order: ${qty} chicken pie(s). ${notes ? 'Notes: ' + notes : ''}`;
    orderSummary.textContent = summaryText;

    const message = `Hello, I'd like to order ${qty} chicken pie(s). ${notes}`;
    openWhatsAppChats(message);
}

// Expose functions to global scope for inline handlers
window.showMessage = showMessage;
window.orderWhatsApp = orderWhatsApp;

// Appointment booking for Nail Services
const SLOTS = ['09:00','10:00','11:00','12:00','13:00','14:00','15:00','16:00','17:00','18:00'];
const bookings = loadBookings(); // { 'YYYY-MM-DD': ['09:00','10:00'] }
const STORAGE_KEY = 'vt_bookings';

function saveBookings(){
    try{
        localStorage.setItem(STORAGE_KEY, JSON.stringify(bookings));
    }catch(e){
        console.warn('Could not save bookings', e);
    }
}

function loadBookings(){
    try{
        const raw = localStorage.getItem(STORAGE_KEY);
        return raw ? JSON.parse(raw) : {};
    }catch(e){
        console.warn('Could not load bookings', e);
        return {};
    }
}

function updateSlotOptions(){
    const dateEl = document.getElementById('apptDate');
    const select = document.getElementById('slotSelect');
    if(!select || !dateEl) return;
    const date = dateEl.value;
    select.innerHTML = '';
    if(!date){
        const opt = document.createElement('option');
        opt.value = '';
        opt.textContent = 'Select a date';
        select.appendChild(opt);
        return;
    }

    const taken = bookings[date] || [];
    SLOTS.forEach(slot => {
        const opt = document.createElement('option');
        opt.value = slot;
        opt.textContent = taken.includes(slot) ? `${slot} (Booked)` : slot;
        if(taken.includes(slot)) opt.disabled = true;
        select.appendChild(opt);
    });
}

function bookAppointment(){
    const dateEl = document.getElementById('apptDate');
    const select = document.getElementById('slotSelect');
    const nameEl = document.getElementById('clientName');
    const summaryEl = document.getElementById('appointmentSummary');
    if(!dateEl || !select || !summaryEl) return;

    const date = dateEl.value;
    const slot = select.value;
    if(!date || !slot){
        alert('Please choose a date and a time slot.');
        return;
    }

    const taken = bookings[date] || [];
    if(taken.includes(slot)){
        alert('Selected slot is already booked. Pick another slot.');
        updateSlotOptions();
        return;
    }

    if(taken.length >= SLOTS.length){
        alert('No available slots for this date.');
        return;
    }

    taken.push(slot);
    bookings[date] = taken;
    saveBookings();
    updateSlotOptions();

    const name = nameEl && nameEl.value ? nameEl.value.trim() : '';
    const summaryText = `Appointment booked: ${date} at ${slot}${name ? ' for ' + name : ''}`;
    summaryEl.textContent = summaryText;

    const message = `Hello, I'd like to book an appointment on ${date} at ${slot}${name ? ' for ' + name : ''}.`;
    openWhatsAppChats(message);
}

let slideIndex = 0;

function showSlides(index) {
    const slides = document.querySelectorAll('.slide');
    const dots = document.querySelectorAll('.dot');
    if (!slides.length) return;
    if (index >= slides.length) slideIndex = 0;
    if (index < 0) slideIndex = slides.length - 1;
    slides.forEach((slide, idx) => {
        slide.classList.toggle('active', idx === slideIndex);
    });
    dots.forEach((dot, idx) => {
        dot.classList.toggle('active', idx === slideIndex);
    });
}

function plusSlides(step) {
    slideIndex += step;
    showSlides(slideIndex);
}

function currentSlide(n) {
    slideIndex = n - 1;
    showSlides(slideIndex);
}

function startSlideshow(){
    showSlides(slideIndex);
    setInterval(() => {
        slideIndex += 1;
        showSlides(slideIndex);
    }, 4000);
}

// Wire up date change listener
document.addEventListener('DOMContentLoaded', () => {
    const dateEl = document.getElementById('apptDate');
    if(dateEl) dateEl.addEventListener('change', updateSlotOptions);
    updateSlotOptions();
    startSlideshow();
});

function submitReview(){
    const reviewInput = document.getElementById('reviewText');
    const confirmation = document.getElementById('reviewConfirmation');
    if(!reviewInput) return;

    const review = reviewInput.value.trim();
    if(!review){
        alert('Please enter your review before sending.');
        return;
    }

    const message = `Private feedback for owner:\n${review}`;
    openWhatsAppChats(message);

    if(confirmation) {
        confirmation.textContent = 'Review ready to send to the owner. Please complete the WhatsApp messages.';
    }
}

window.bookAppointment = bookAppointment;
window.submitReview = submitReview;
