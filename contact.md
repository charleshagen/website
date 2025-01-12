---
layout: page
title:  Contact
---

<div class="container">
  <form target="_blank" action="https://formsubmit.co/f896db85ddbc788ab12f5d88aa4a8a1d" method="POST">
    <input type="hidden" name="_template" value="box">
    <!-- <input type="hidden" name="_captcha" value="false"> -->
    <input type="hidden" name="_next" value="https://www.nightphotons.com/">
    <div class="form-group">
      <div class="form-row">
        <div class="col">
            <p>Name:</p>
          <input type="text" name="name" class="form-control" placeholder="Name" required>
        </div>
        <div class="col">
            <p>Email:</p>
            <input type="email" name="email" class="form-control" placeholder="Email Address" required>
        </div>
      </div>
    </div>
    <div class="form-group">
        <p>Subject:</p>
        <textarea placeholder="Subject" class="form-control" name="_subject" rows="1" required></textarea>
    </div>
    <div class="form-group">
        <p>Message:</p>
        <textarea placeholder="Your Message" class="form-control" name="message" rows="8" required></textarea>
    </div>
    <button type="submit" class="btn btn-lg btn-dark btn-block">Send</button>
  </form>
</div>

<style>
  .container form {
    margin-bottom:0;
  }
  .container {
    background-color: rgba(110,110,110,0.15);
    padding: 1.5rem;
  }
  .container button {
    font-size: 1rem;
    width: 40%;
    margin-left: auto;
    margin-right: auto;
    background-color: rgba(128,128,128,0.3);
    font-weight: normal;
  }
  .container p {
    margin-bottom: 0.2rem;
  }
  .container textarea::placeholder {
    opacity: 40%;
  }
  .container input::placeholder {
    opacity: 40%;
  }
</style>