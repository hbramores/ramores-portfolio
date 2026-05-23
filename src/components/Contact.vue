<script setup>
import { ref, onMounted, onBeforeUnmount } from 'vue';
import { Notyf } from 'notyf';
import 'notyf/notyf.min.css';

const notyf = new Notyf();

const WEB3FORMS_ACCESS_KEY = '94c6c9a5-b670-4f40-bfba-a7d4e4fe96a7';
const SITE_KEY = '6LfrjvgsAAAAAIsPcL8OiU3tMEvUtbou4nQl7xyr';
const subject = 'New message from Portfolio Contact Form';

const name = ref('');
const email = ref('');
const phone = ref('');
const inquiryType = ref('');
const message = ref('');
const isLoading = ref(false);

const recaptchaContainer = ref(null);
const recaptchaWidgetId = ref(null);
const recaptchaToken = ref('');
let recaptchaLoadInterval = null;

function onRecaptchaSuccess(token) {
	recaptchaToken.value = token;
}

function onRecaptchaExpired() {
	recaptchaToken.value = '';
}

function renderRecaptcha() {
	if (!window.grecaptcha || !recaptchaContainer.value || recaptchaWidgetId.value !== null) {
		return;
	}

	recaptchaWidgetId.value = window.grecaptcha.render(recaptchaContainer.value, {
		sitekey: SITE_KEY,
		size: 'normal',
		callback: onRecaptchaSuccess,
		'expired-callback': onRecaptchaExpired
	});
}

function resetRecaptcha() {
	if (recaptchaWidgetId.value !== null && window.grecaptcha) {
		window.grecaptcha.reset(recaptchaWidgetId.value);
		recaptchaToken.value = '';
	}
}

function resetForm() {
	name.value = '';
	email.value = '';
	phone.value = '';
	inquiryType.value = '';
	message.value = '';
}

async function submitForm() {
	if (!recaptchaToken.value) {
		notyf.error('Please verify that you are not a robot.');
		return;
	}

	isLoading.value = true;

	try {
		const response = await fetch('https://api.web3forms.com/submit', {
			method: 'POST',
			headers: {
				'Content-Type': 'application/json',
				Accept: 'application/json'
			},
			body: JSON.stringify({
				access_key: WEB3FORMS_ACCESS_KEY,
				subject,
				name: name.value,
				email: email.value,
				phone: phone.value,
				inquiry_type: inquiryType.value,
				message: message.value
			})
		});

		const result = await response.json();

		if (result.success) {
			notyf.success('Message sent!');
			resetForm();
		} else {
			notyf.error(result.message || 'Failed to send message');
		}
	} catch (error) {
		console.error(error);
		notyf.error('Failed to send message');
	} finally {
		isLoading.value = false;
		resetRecaptcha();
	}
}

onMounted(() => {
	recaptchaLoadInterval = setInterval(() => {
		if (window.grecaptcha && window.grecaptcha.render) {
			renderRecaptcha();
			clearInterval(recaptchaLoadInterval);
			recaptchaLoadInterval = null;
		}
	}, 100);
});

onBeforeUnmount(() => {
	if (recaptchaLoadInterval) {
		clearInterval(recaptchaLoadInterval);
	}
});
</script>

<template>
	<div id="contact" class="contact p-5">
		<h1 class="text-center">CONTACT</h1>
		<h3 class="section-desc">Let's collaborate and turn your ideas into powerful digital experiences.</h3>

		<div class="row">
			<div class="col-md-6 p-5">
				<h1>Let's build your digital future!</h1>
				<p>Open for collaborations and projects. Let's build modern, functional, and impactful digital experiences together.</p>
				<h3>Social media</h3>
				<div class="d-flex">
					<a href="https://www.linkedin.com/in/hana-ramores-22069a332/" target="_blank">
						<img src="/images/linkedin.png" alt="LinkedIn">
					</a>

					<a href="https://profile.indeed.com/?hl=en_PH&co=PH&from=gnav-jobseeker-profile--profile-one-frontend" target="_blank">
						<img src="/images/indeed.png" alt="Indeed">
					</a>

					<a href="https://jobseeker.kalibrr.com/candidate/profile" target="_blank">
						<img src="/images/kalibrr.png" alt="Kalibrr">
					</a>

					<a href="https://www.facebook.com/hana.ramores.3" target="_blank">
						<img src="/images/facebook.png" alt="Facebook">
					</a>
				</div>
			</div>

			<div class="col-md-6 p-5">
				<div class="contact-card">
					<h2 class="text-center mb-4">Message now!</h2>

					<form @submit.prevent="submitForm">
						<div class="row g-3">
							<div class="col-12">
								<input
									v-model="name"
									type="text"
									class="form-control custom-input"
									placeholder="Full Name"
									required
								>
							</div>

							<div class="col-12 col-md-6">
								<input
									v-model="email"
									type="email"
									class="form-control custom-input"
									placeholder="Email Address"
									required
								>
							</div>

							<div class="col-12 col-md-6">
								<input
									v-model="phone"
									type="text"
									class="form-control custom-input"
									placeholder="Phone Number"
								>
							</div>

							<div class="col-12">
								<select v-model="inquiryType" class="form-select custom-input" required>
									<option value="" disabled>Inquiry Type</option>
									<option>Web Development</option>
									<option>UI/UX Design</option>
									<option>Collaboration</option>
									<option>Other</option>
								</select>
							</div>

							<div class="col-12">
								<textarea
									v-model="message"
									class="form-control custom-input"
									rows="4"
									placeholder="Your Message"
									required
								></textarea>
							</div>

							<div class="col-12 captcha-wrap">
								<div ref="recaptchaContainer"></div>
							</div>

							<div class="col-12 text-center">
								<button type="submit" class="send-btn" :disabled="isLoading">
									{{ isLoading ? 'Sending...' : 'Send Message' }}
								</button>
							</div>
						</div>
					</form>
				</div>
			</div>
		</div>
	</div>
</template>
