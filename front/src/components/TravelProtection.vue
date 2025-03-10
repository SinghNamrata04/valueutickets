<template>
  <div class="travel-protection">
    <h2 class="title">Travel Protection Plan</h2>
    <div class="protection-container">
      <div class="protection-options">
        <div class="option">
          <div class="icon-heading">
            <img src="/images/cancel.png" alt="Trip Cancellation" class="icon" />
            <h3 class="heading">Trip Cancellation</h3>
          </div>
          <p class="description">Covers cancellations due to unexpected illnesses (like Covid-19) and more.</p>
        </div>
        <div class="option">
          <div class="icon-heading">
            <img src="/images/clock.png" alt="Travel Delay" class="icon" />
            <h3 class="heading">Travel Delay</h3>
          </div>
          <p class="description">Delayed while en route to or from your trip? This can help cover your costs.</p>
        </div>
        <div class="option">
          <div class="icon-heading">
            <img src="/images/bandage.png" alt="Medical Expenses" class="icon" />
            <h3 class="heading">Medical Expenses</h3>
          </div>
          <p class="description">Injure yourself on your trip? This can help cover medical expenses.</p>
        </div>
        <div class="option">
          <div class="icon-heading">
            <img src="/images/healthcare.png" alt="Medical Evacuation" class="icon" />
            <h3 class="heading">Medical Evacuation</h3>
          </div>
          <p class="description">This can help in the most urgent medical situations.</p>
        </div>
      </div>

      <div class="selection-box">
        <div class="selection-container">
          <label class="selection">
            <input type="radio" name="protection" />
            <div class="text">
              <strong>Yes, I want Cancellation Protection Plan for my flights.</strong>
              <p>
                The quoted price for the travel protection plan includes the plan premium and a fee for non-insurance assistance services.
                You may obtain information on the plan fees by emailing <a href="mailto:support@flightschannel.com">support@flightschannel.com</a>.
              </p>
            </div>
            <div class="price-text">
              <p class="price">$305</p>
              <p class="per-person">Per Person</p>
            </div>
          </label>
        </div>

        <div class="selection-container">
          <label class="selection selected">
            <input type="radio" name="protection" checked />
            <div class="text">
              <strong>No, I'm willing to risk my flight.</strong>
              <p>
                I understand by declining this coverage that I may be responsible for cancellation fees and delay expenses personally or through alternate coverage.
              </p>
            </div>
            <div class="price-text">
              <p class="price">$000.00</p>
              <p class="per-person">Per Person</p>
            </div>
          </label>
        </div>
      </div>
      
      <p class="note">*Travel Protection Plan Is Non-Refundable Once Purchased.</p>
    </div>
  </div>
</template>

<style>
.travel-protection {
  max-width: 1100px;
  margin: auto;
  margin-bottom: 40px;
  padding: 20px;
  background: #ffffff;
  border-radius: 10px;
  font-family: Arial, sans-serif;
  box-shadow: 0 2px 15px rgba(0, 0, 0, 0.1);
}

.title {
  font-size: 18px;
  font-weight: bold;
  margin-bottom: 20px;
  text-align: left;
}

.protection-container {
  background: #f9fafb;
  padding: 20px;
  border-radius: 8px;
  border: 2px solid #d1d5db;
}

.protection-options {
  display: flex;
  flex-wrap: wrap;
  justify-content: space-between;
  padding-bottom: 20px;
}

.option {
  width: 48%;
  margin-bottom: 20px;
}

.icon-heading {
  display: flex;
  align-items: center;
  gap: 12px;
  margin-left: 50px;
}

.icon {
  width: 35px;
  height: 35px;
}

.heading {
  margin: 0;
  font-size: 18px;
  font-weight: bold;
}

.description {
  font-size: 14px;
  margin-top: 8px;
  margin-left: 100px;
  width: 300px; /* Set a fixed width */
}

.selection-box {
  margin-top: -30px;
}

.selection-container {
  background: #ffffff;
  padding: 15px;
  border-radius: 8px;
  border: 2px solid #d1d5db;
  margin-bottom: 15px;
  width: 100%; /* Ensure it takes the full available width */
  box-sizing: border-box; /* Include padding in the width calculation */
}

.selection {
  display: flex;
  align-items: center;
  background: #e1f8ed;
  padding: 15px;
  border-radius: 8px;
  width: 100%;
  cursor: pointer;
  position: relative;
  border: none;
  box-sizing: border-box; /* Ensure the padding doesn't cause overflow */
}

.selection input {
  width: 30px;
  height: 30px;
  margin-right: 20px;
  position: absolute;
  left: 15px;
}

.text {
  flex-grow: 1;
  text-align: left;
  margin-left: 60px;
  max-width: 65%;
}

.price-text {
  display: flex;
  align-items: baseline;
  margin-left: auto;
  margin-right: 20px;
}

.price {
  font-size: 18px;
  font-weight: bold;
  color: #0073e6;
}

.per-person {
  font-size: 14px;
  color: #5b5b5b;
  margin-left: 10px;
}

.note {
  font-size: 16px;
  color: #5b5b5b;
  margin-top: 15px;
  text-align: left;
}

/* Mobile-responsive adjustments (only for mobile screens) */
@media (max-width: 768px) {
  .protection-options {
    flex-direction: column;
    align-items: center; /* Centers the content */
  }

  .option {
    width: 90%; /* Makes each option more centered on mobile */
    margin-bottom: 20px;
  }

  .icon-heading {
    margin-left: 0; /* Reset margin for mobile */
    justify-content: center; /* Centers the icon and heading */
  }

  .icon {
    width: 30px; /* Adjust icon size */
    height: 30px;
  }

  .description {
    margin-left: 0; /* Reset left margin for mobile */
    width: auto; /* Allow description to take full width */
    margin-top: 8px;
    text-align: center; /* Centers the description text */
  }

  .selection-container {
    padding: 10px; /* Reduces padding for mobile */
    width: 100%; /* Ensures the selection container takes full width */
  }

  .selection {
    flex-direction: column; /* Stacks the radio button and text vertically */
    align-items: center; /* Aligns the radio button and text at the center */
    padding: 15px 10px; /* Adjust padding for mobile */
  }

  .selection input {
    margin: 0 0 10px 0; /* Adds margin below radio button */
  }

  .text {
    text-align: left; /* Centers the text below the circle */
    margin-top: 5px; /* Adds space between text and circle */
    word-wrap: break-word; /* Prevents text overflow */
  }

  .price-text {
    align-items: center;
  }

  .price {
    margin-bottom: 0;
  }

  .per-person {
    text-align: center; /* Centers the "Per Person" text */
    margin-top: 5px;
  }

  .note {
    text-align: center; /* Centers the note text */
  }
}
</style>
