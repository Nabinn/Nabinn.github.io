---
layout: page
title: Contact Me
show-avatar: true
---
<!--
<form id="contactform" method="POST">

<input type="hidden" name="_next" value="thanks" />
	<input type="hidden" name="_subject" value="Website contact" />

	<input type="text" name="name" placeholder="Your name">
    <input type="email" name="_replyto" placeholder="Your email">
   
    <textarea name="message" placeholder="Your message"></textarea>
    <input type="text" name="_gotcha" style="display:none" />
    <input type="submit" value="Send">
</form>
-->




<div class="container">
    <div class="row">
        <div class="col-md-8">
            <div class="well well-sm">
                <form id="contactform" method="POST">
                <input type="hidden" name="_next" value="thanks" />
				<input type="hidden" name="_subject" value="Website contact" />
				<input type="text" name="_gotcha" style="display:none" />
                <div class="row">
                    <div class="col-md-6">
                        <div class="form-group">
                            <label for="name">
                                Name</label>
                            <input type="text" name="name" class="form-control" id="name" placeholder="Your name" required="required" />
                        </div>
                        <div class="form-group">
                            <label for="email">
                                Email Address</label>
                            <div class="input-group">
                                <span class="input-group-addon"><span class="glyphicon glyphicon-envelope"></span>
                                </span>
                                <input type="email" name="_replyto" class="form-control" id="email" placeholder="Your email" required="required" />

                                </div>
                        </div>
                        
                    </div>
                    <div class="col-md-6">
                        <div class="form-group">
                            <label for="name">
                                Message</label>
                            <textarea name="message" id="message" class="form-control" rows="9" cols="25" required="required"
                                placeholder="Your message"></textarea>
                        </div>
                    </div>
                    <div class="col-md-12">
                        <button type="submit" class="btn btn-primary pull-right" id="btnContactUs" style="background: black; active: red;">
                            Send Message</button>
                    </div>
                </div>
                </form>
            </div>
        </div>

    </div>
</div>

<script>
    var contactform =  document.getElementById('contactform');
    contactform.setAttribute('action', '//formspree.io/' + 'nabin99sharma' + '@' + 'gmail' + '.' + 'com');
</script>