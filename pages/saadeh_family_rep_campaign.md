---
layout: default
title: Saadeh
permalink: /saadeh/
---

<script>
  async function submitForm() {
    const zipCode = document.getElementById('zip').value;
    
    const apiUrl = `https://yra6hejx6k.execute-api.us-east-1.amazonaws.com/dev/get-representatives?zipcode=${zipCode}`;
    
    try {
      const response = await fetch(apiUrl);
      const data = await response.json();

      // Parsing the JSON, logging the value of the "email" key, and generating a mailto link
      data.forEach(item => {
        if (item.email) {
          const email = item.email;
          const subject = "Constituent Demand - HELP DR. SAADEH'S FAMILY";
          const body = `ADDRESS:

I am writing to urgently bring to your attention a matter of utmost importance that requires immediate action. I came across this report on Fox 2 Detroit, https://www.fox2detroit.com/video/1376036.

This is a horrific situation to be in. Dr. Yamaan Saadeh is a respected member of the Ann Arbor community and is currently embroiled in a harrowing situation as he endeavors to secure the safety and well-being of his family trapped in a conflict zone. Their lives are in peril, and swift intervention is crucial to prevent further tragedy.

The situation on the ground in Gaza demands an IMMEDIATE ceasefire. The ongoing conflict has placed innocent civilians, including children and the elderly, in grave danger. Lives are being shattered, families torn apart, and the humanitarian crisis is escalating by the hour. It is imperative that steps be taken to halt the violence and ensure the safety of all individuals caught in this turmoil.

As a constituent within your jurisdiction, I implore you to leverage your position and resources to extend a helping hand to Dr. Saadeh and his family. Your influence and connections can make a significant difference in facilitating their safe passage out of the conflict zone. Every moment is precious, and your prompt intervention can be the lifeline that these vulnerable individuals desperately need.

I demand, as your constituent, that you take immediate action to reach out to relevant authorities, utilize diplomatic channels, and exhaust all available means to provide assistance and ensure the safe evacuation of Dr. Saadeh's family. Time is of the essence, and any delay in your intervention could have dire consequences.

I urge you to act swiftly and decisively to bring about a ceasefire and to offer the necessary support and aid to evacuate innocent civilians from this conflict zone. The lives of Dr. Saadeh's family, along with countless others, depend on the urgency and efficacy of your response.

I will be eagerly awaiting your prompt action and update on the measures taken to address this critical situation. Your leadership and intervention in this matter will not only reflect compassion and responsibility but will also serve as a beacon of hope for those trapped in unimaginable circumstances.

Thank you for your immediate attention to this pressing matter. I trust that you will act with the urgency and empathy required to alleviate the suffering of innocent civilians.

Sincerely,
          `;
          const mailtoLink = `mailto:${email}?subject=${encodeURIComponent(subject)}&body=${encodeURIComponent(body)}`;

          // Open the mailto link in a new tab
          window.location.href = mailtoLink
        }
      });
    } catch (error) {
      console.error('Error fetching data:', error);
    }
  }
</script>

## Saadeh Family Support Representative Email Campaign

This form will generate a strongly worded letter to the representative of your zipcode. It will open in your email client and you have the ability to modify the content of the email before sending.

**MAKE SURE TO INCLUDE YOUR POSTAL ADDRESS! REPRESENTATIVES WILL ONLY ADDRESS EMAILS FROM THEIR OWN DISTRICTS. Make sure to add your name at the bottom.**

<div style="display: flex; justify-content: center; align-items: center; height: 30vh;">
  <form action="/submit" method="post" style="border: 2px solid #ccc; padding: 20px; border-radius: 8px; max-width: 400px; width: 100%;">
    <label for="zip" style="display: block; margin-bottom: 5px;">Zip Code:</label>
    <input type="text" id="zip" name="zip" style="width: calc(100% - 18px); padding: 8px; margin-bottom: 10px; border-radius: 5px; border: 1px solid #ccc;">

    <button type="button" onclick="submitForm()" style="display: block; margin: 0 auto; background-color: #007bff; color: white; font-weight: bold; width: 200px; padding: 8px; border-radius: 5px; border: none;">
      Generate Email
    </button>
  </form>
</div>

Learn more about the issue: [https://www.fox2detroit.com/video/1376036](https://www.fox2detroit.com/video/1376036).