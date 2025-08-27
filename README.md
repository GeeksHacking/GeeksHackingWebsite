# GeeksHacking Website

A simple, static HTML website for the GeeksHacking community.

## How to Update Content

To add a new event to the website, you need to add the event's images and create a data entry for it.

### 1. Add Event Images

- Create a new folder for your event inside `assets/images/GeeksHacking/Events/`. The folder name should match the event name precisely (e.g., `My Awesome Workshop`).
- Add a `banner.jpg` to this new folder. This image will be used on the homepage slider.
- If you have more photos, create a subfolder named `compressed` and add the rest of the event photos there.

### 2. Add Event Data

- Open the file `assets/js/data.js`.
- Add a new JavaScript object to the `events` array.
- The object should follow this structure:

```javascript
{
  name: "My Awesome Workshop",
  photos: 10, // Number of photos in the 'compressed' folder
  description: "A brief description of the event."
}
```

## Deployment

This website is hosted on a virtual server from **UpCloud**.

Deployment is fully automated using **GitHub Actions**. Every push to the `master` branch will automatically trigger a workflow that connects to the server and pulls the latest changes.
