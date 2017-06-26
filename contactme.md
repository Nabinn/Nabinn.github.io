---
layout: page
title: About me
subtitle: (under construction)
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