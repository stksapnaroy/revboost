window.addEventListener("load", function () {
  // Check for hidden fields
  const hiddenFields = document.querySelectorAll(
    '[data-wf-hs-form] input[type="hidden"], [data-webflow-hubspot-api-form-url] input[type="hidden"]'
  );

  // If there are hidden fields, update the values
  if (hiddenFields.length > 0) {
    hiddenFields.forEach((field) => {
      switch (field.name) {
        case "hutk":
          const cookies = document.cookie.split(";");
          const cookieMap = {};

          cookies.forEach((cookie) => {
            const [name, value] = cookie.trim().split("=");
            cookieMap[name] = value;
          });

          const hubspotCookie = cookieMap["hubspotutk"];
          field.value = hubspotCookie;
          break;
        case "pageUri":
          field.value = window.location.href;
          break;
        case "pageName":
          field.value = document.title;
          break;
        case "pageId":
          field.value = window.location.pathname;
          break;
        case "ipAddress":
          break;
        default:
          break;
      }
    });
  }

  const webflowHubSpotForms = document.querySelectorAll("[data-wf-hs-form]");
  if (webflowHubSpotForms.length > 0) {
    webflowHubSpotForms.forEach((form) => {
      form.addEventListener("submit", (event) => {
        event.preventDefault();
        const formData = new FormData(form);
        fetch(form.action, {
          method: form.method,
          body: formData,
        })
          .then((response) => response.json())
          .then((data) => {
            if ("redirectUri" in data) {
              window.location.href = data.redirectUri;
            }

            if ("inlineMessage" in data) {
              const message = document.createElement("div");
              message.style.marginTop = "1rem";
              message.style.marginBottom = "1rem";
              message.innerHTML = data.inlineMessage;
              form.appendChild(message);
              message.scrollIntoView({ behavior: "smooth", block: "center" });
            }
          })
          .catch((error) => console.error(error));
      });
    });
  }

  const webflowForms = document.querySelectorAll(
    "[data-webflow-hubspot-api-form-url]"
  );
  if (webflowForms.length > 0) {
    webflowForms.forEach((form) => {
      form.addEventListener("submit", (event) => {
        event.preventDefault();
        const formData = new FormData(form);
        form.querySelectorAll("[data-wfhsfieldname]").forEach((field) => {
          if (field.type === "file") {
            formData.set(field.dataset.wfhsfieldname, field.files[0]);
          } else {
            formData.set(field.dataset.wfhsfieldname, field.value);
          }
          formData.delete(field.name);
        });

        fetch(form.dataset.webflowHubspotApiFormUrl, {
          method: "POST",
          body: formData,
        })
          .then((response) => response.json())
          .catch((error) => console.error(error));
      });
    });
  }
});
