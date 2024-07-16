# Redirect to SMS Script

The script is designed to redirect users to different URLs based on their device type. The script also checks if the user is logged into a WordPress site and prevents redirection in that case, making it much easier to modify 😛

## Overview

The script performs the following actions:
1. Checks if the user is logged into a WordPress site by looking for specific elements.
2. If the user is logged in, displays a message and stops further execution.
3. If the user is not logged in, hides all content on the page except for a message indicating redirection.
4. Redirects the user to an SMS URL or a fallback URL based on their device type.

## Code Explanation

```html
<div id="user-agent-output"></div>
<script>
    document.addEventListener('DOMContentLoaded', () => {

        // Get the output container
        const userAgentOutput = document.querySelector('#user-agent-output');

        // Check to see if user is logged in
        const beaverBuilderBar = document.querySelector('.fl-builder-bar');
        const wpAdminBar = document.querySelector('#wpadminbar');
        const isUserLoggedIn = !!beaverBuilderBar || !!wpAdminBar;

        // If user is logged in, abort! We don't want to redirect!
        if (isUserLoggedIn) {
            userAgentOutput.innerHTML = `You are logged into WordPress. Redirect will not fire 🚀`;
            return;
        } else {
            // Hide everything except the message
            document.body.innerHTML = '';
            const message = document.createElement('div');
            message.innerHTML = `
                <p>Redirecting to SMS...</p>
            `;
            document.body.appendChild(message);
        }

        setTimeout(() => {
            let userAgent = navigator.userAgent || navigator.vendor || window.opera;
            const smsBody = `Check out this kiosk I found! https://intellectualtechnologyinc.com/self-service/`;

            if (/iPad|iPhone|iPod/.test(userAgent) && !window.MSStream) {
                window.location.href = `sms:&body=${smsBody}`;
            } else if (/android/i.test(userAgent)) {
                window.location.href = `sms:?body=${smsBody}`;
            } else {
                window.location.href = 'https://intellectualtechnologyinc.com';
            }
        }, 1000);
    });
</script>
