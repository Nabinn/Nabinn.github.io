---
layout: page
title: Search
show-avatar: true
---
    
search with google2
<style type="text/css">
	
Skip to content
 
Search…
All gists
GitHub
New gist
@nabinn
  Star 1
  Fork 1
  @brandonrosagebrandonrosage/google.css
Created 5 years ago
Embed  
<script src="https://gist.github.com/brandonrosage/4409806.js"></script>
  Download ZIP
 Code  Revisions 1  Stars 1  Forks 1
Stylesheet intended to override Google's custom search styles on the OIC website.
Raw
 google.css
#cse-search-form {
   position: relative;
   -webkit-border-radius: 3px;
   -moz-border-radius: 3px;
   border-radius: 3px;
   background: #f2f2f2;
   padding: 3px;  
}

#dialog-container #cse-search-form {
   padding: 0;
   margin: 5px 3.125% 15px;
}

#cse-search-form form.gsc-search-box {
   margin: 0;
}

#cse-search-form table.gsc-search-box {
   border-collapse: collapse;
   margin: 0;
}

#cse-search-form .gsc-search-box-tools .gsc-search-box .gsc-input {
	padding: 0;
}

.gsc-search-box-tools .gsc-search-box .gsc-input .gsc-input-box,
.gsc-search-box-tools .gsc-search-box .gsc-input .gsc-input-box-focus {
   height: 24px;  
-webkit-border-top-left-radius: 3px;
-webkit-border-bottom-left-radius: 3px;
-moz-border-radius-topleft: 3px;
-moz-border-radius-bottomleft: 3px;
border-top-left-radius: 3px;
border-bottom-left-radius: 3px;
   border: 1px solid #dadada;
   border-right: none;
   padding: 3px 10px;
}

.gsc-search-box .gsc-input > input:focus, 
.gsc-input-box-focus,
.gsc-input-box-hover {
  -moz-box-shadow: none;
  -webkit-box-shadow: none;
  box-shadow: none;
}

#cse-search-form form.gsc-search-box input.gsc-input {
   font-size: 13px;
   background: none repeat scroll 0% 0% white !important;
   padding: 2px 0;
}

.gsib_a {
   padding: 4px 2px;
}

.gsst_a .gscb_a {
   color: #008fd5;
}

.gsc-search-box-tools .gsc-search-box .gsc-search-button:before {
	position: absolute;
	top: 10px;
	right: 10px;
	z-index: 100;
	font-family: 'oic-icons';
	font-style: normal;
	speak: none;
	content: "\30";
   font-size: 1.2em;
}

#dialog-container .gsc-search-box-tools .gsc-search-box .gsc-search-button:before {
   top: 7px;
}

#cse-search-form.active .gsc-search-box-tools .gsc-search-box .gsc-search-button:before {
   display: none;
   right: -9999px;
   color: #fff;
}

.gsc-search-box-tools .gsc-search-box .gsc-search-button input.gsc-search-button {
   display: block;
	width: 12px;
	height: 12px;
-webkit-border-top-right-radius: 3px;
-webkit-border-bottom-right-radius: 3px;
-moz-border-radius-topright: 3px;
-moz-border-radius-bottomright: 3px;
border-top-right-radius: 3px;
border-bottom-right-radius: 3px;
-webkit-border-top-left-radius: 0px;
-webkit-border-bottom-left-radius: 0px;
-moz-border-radius-topleft: 0px;
-moz-border-radius-bottomleft: 0px;
border-top-left-radius: 0px;
border-bottom-left-radius: 0px;
   border: 1px solid #dadada;
   border-left: none;
	background-color: #fff;
	filter: none;
	padding: 9px;
	margin: 0;
}

#dialog-container .gsc-search-box-tools .gsc-search-box .gsc-search-button input.gsc-search-button {
   top: -5px;
}

#cse-search-form.active .gsc-search-box-tools .gsc-search-box .gsc-search-button input.gsc-search-button {
   border: 1px solid #0376ae;
	background: #008FD5;	
}

#cse-search-form.active .gsc-search-box-tools .gsc-search-box .gsc-search-button input.gsc-search-button:hover {
	background: #0376AE;	
}
 @nabinn
  
            
 
Write  Preview

Leave a comment
Attach files by dragging & dropping,  Choose Files selecting them, or pasting from the clipboard.
 Styling with Markdown is supported
Comment
Contact GitHub API Training Shop Blog About
© 2017 GitHub, Inc. Terms Privacy Security Status Help
	
</style>
<div>
<script>
  (function() {
    var cx = '012709239189562225172:bhgo_xtsn4k';
    var gcse = document.createElement('script');
    gcse.type = 'text/javascript';
    gcse.async = true;
    gcse.src = 'https://cse.google.com/cse.js?cx=' + cx;
    var s = document.getElementsByTagName('script')[0];
    s.parentNode.insertBefore(gcse, s);
  })();
</script>
<gcse:search></gcse:search>
</div>