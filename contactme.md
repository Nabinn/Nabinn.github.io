---
layout: page
title: Contact Me
show-avatar: true
---
<form id="contactform" method="POST">

<input type="hidden" name="_next" value="thanks" />
	<input type="hidden" name="_subject" value="Website contact" />

	<input type="text" name="name" placeholder="Your name">
    <input type="email" name="_replyto" placeholder="Your email">
   
    <textarea name="message" placeholder="Your message"></textarea>
    <input type="text" name="_gotcha" style="display:none" />
    <input type="submit" value="Send">
</form>
<script>
    var contactform =  document.getElementById('contactform');
    contactform.setAttribute('action', '//formspree.io/' + 'nabin99sharma' + '@' + 'gmail' + '.' + 'com');
</script>


<div class="container">
  <form action="#">

    <label for="fname">First Name</label>
    <input type="text" id="fname" name="firstname" placeholder="Your name..">

    <label for="lname">Last Name</label>
    <input type="text" id="lname" name="lastname" placeholder="Your last name..">

    <label for="country">Country</label>
    <select id="country" name="country">
      <option value="australia">Australia</option>
      <option value="canada">Canada</option>
      <option value="usa">USA</option>
    </select>

    <label for="subject">Subject</label>
    <textarea id="subject" name="subject" placeholder="Write something.." style="height:200px"></textarea>

    <input type="submit" value="Submit">

  </form>
</div>