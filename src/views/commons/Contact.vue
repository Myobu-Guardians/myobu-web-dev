<template>
    <section
        id="contact"
        class="contact-us ptb-100"
        v-bind:class="isGray ? 'gray-light-bg' : ''"
    >
        <div class="container">
            <div class="row">
                <div
                    class="col-12 pb-3 message-box d-done"
                    v-bind:class="{
                        'd-none': !isSuccess && !hasError,
                        'd-block': isSuccess || hasError,
                    }"
                >
                    <div
                        class="alert"
                        v-bind:class="{
                            'alert-danger': hasError,
                            'alert-success': isSuccess,
                        }"
                    >
                        {{ alertText }}
                    </div>
                </div>
                <div class="col-md-5">
                    <div class="section-heading">
                        <h3>Contact with us</h3>
                        <p>
                            It's very easy to get in touch with us. Just use the
                            contact form or pay us a visit for a coffee at the
                            office. Dynamically innovate competitive technology
                            after an expanded array of leadership.
                        </p>
                    </div>
                    <div class="footer-address">
                        <h6><strong>Head Office</strong></h6>
                        <p>121 King St, Melbourne VIC 3000, Australia</p>
                        <ul>
                            <li><span>Phone: +61 2 8376 6284</span></li>
                            <li>
                                <span
                                    >Email :
                                    <a href="mailto:hello@yourdomain.com"
                                        >hello@yourdomain.com</a
                                    ></span
                                >
                            </li>
                        </ul>
                    </div>
                </div>
                <div class="col-md-7">
                    <form
                        v-on:submit="submit"
                        id="contactForm"
                        class="contact-us-form"
                    >
                        <h5>Reach us quickly</h5>
                        <div class="row">
                            <div class="col-sm-6 col-12">
                                <div class="form-group">
                                    <input
                                        type="text"
                                        class="form-control"
                                        name="name"
                                        v-model="name"
                                        placeholder="Enter name"
                                        required="required"
                                    />
                                </div>
                            </div>
                            <div class="col-sm-6 col-12">
                                <div class="form-group">
                                    <input
                                        type="email"
                                        class="form-control"
                                        name="email"
                                        v-model="email"
                                        placeholder="Enter email"
                                        required="required"
                                    />
                                </div>
                            </div>
                        </div>
                        <div class="row">
                            <div class="col-sm-6 col-12">
                                <div class="form-group">
                                    <input
                                        type="text"
                                        name="phone"
                                        value=""
                                        class="form-control"
                                        v-model="phone"
                                        placeholder="Your Phone"
                                        required="required"
                                    />
                                </div>
                            </div>
                            <div class="col-sm-6 col-12">
                                <div class="form-group">
                                    <input
                                        type="text"
                                        name="company"
                                        value=""
                                        size="40"
                                        class="form-control"
                                        v-model="company"
                                        placeholder="Your Company"
                                        required="required"
                                    />
                                </div>
                            </div>
                        </div>
                        <div class="row">
                            <div class="col-12">
                                <div class="form-group">
                                    <textarea
                                        name="message"
                                        v-model="message"
                                        class="form-control"
                                        rows="7"
                                        cols="25"
                                        placeholder="Message"
                                        required="required"
                                    ></textarea>
                                </div>
                            </div>
                        </div>
                        <div class="row">
                            <div class="col-sm-12 mt-3">
                                <button
                                    type="submit"
                                    class="btn solid-btn disabled"
                                    id="btnContactUs"
                                    style="pointer-events: all; cursor: pointer"
                                >
                                    Send Message
                                </button>
                            </div>
                        </div>
                    </form>
                    <p class="form-message"></p>
                </div>
            </div>
        </div>
    </section>
</template>

<script>
export default {
    name: "Contact",
    props: {
        isGray: {
            type: Boolean,
            default: false,
        },
    },
    data() {
        return {
            email: "",
            message: "",
            name: "",
            phone: "",
            company: "",
            alertText: "",
            hasError: false,
            isSuccess: false,
        };
    },
    methods: {
        submit: function (e) {
            e.preventDefault();

            let formData = new FormData();
            formData.append("name", this.name);
            formData.append("message", this.message);
            formData.append("email", this.email);
            formData.append("phone",this.phone);
            formData.append("company",this.company);

            fetch("/php/contact-form-process.php", {
                body: formData,
                method: "POST",
            })
                .then((response) => {
                    if (response.status === 200) {
                        this.name = "";
                        this.message = "";
                        this.email = "";
                        this.phone = "";
                        this.company = "";
                        this.alertText = "Form submitted successfully";
                        this.hasError = false;
                        this.isSuccess = true;
                    } else {
                        this.alertText =
                            "Found error in the form.Please check again.";
                        this.isSuccess = false;
                        this.hasError = true;
                    }
                })
                .catch((error) => {
                    console.log(error);
                    this.alertText =
                        "Found error in the form. Please check again.";
                    this.isSuccess = false;
                    this.hasError = true;
                });
        },
        mounted() {
            this.alertText = "";
            this.hasError = false;
            this.isSuccess = false;
        },
    },
};
</script>

<style>
</style>