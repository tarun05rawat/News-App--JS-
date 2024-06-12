# News App

This is a simple News App that fetches and displays the latest news headlines using the NewsAPI. The app allows users to search for news articles based on a specific query and displays the results in a user-friendly interface.

## Features

- Fetches and displays top headlines from the United States.
- Allows users to search for news articles by keyword.
- Displays news articles with images, titles, and descriptions.
- Opens the full article in a new tab when a news card is clicked.

## Technologies Used

- HTML
- CSS
- JavaScript
- [NewsAPI](https://newsapi.org/)

## Getting Started

To run this project locally, follow these steps:

1. Clone the repository:
    ```bash
    git clone https://github.com/your-username/news-app.git
    cd news-app
    ```

2. Open `index.html` in your preferred browser.

## Code Explanation

### HTML

The `index.html` file contains the structure of the webpage, including the search input field, search button, and a container for displaying the news articles.

### CSS

The `style.css` file contains basic styling for the app, including styles for the search bar, news cards, and layout.

### JavaScript

The main logic of the app is in the `app.js` file. It includes:

- Fetching top headlines from the NewsAPI.
- Fetching news articles based on a search query.
- Displaying news articles in a card format with images, titles, and descriptions.
- Handling click events to open the full article in a new tab.

#### Example Code Snippet

```javascript
const apiKey = "YOUR_API_KEY_HERE";

const blogContainer = document.getElementById("blog-container");
const searchField = document.getElementById("search-input");
const searchButton = document.getElementById("search-button");

async function fetchRandomNews() {
  try {
    const apiUrl = `https://newsapi.org/v2/top-headlines?country=us&pageSize=10&apikey=${apiKey}`;
    const response = await fetch(apiUrl);
    const data = await response.json();
    return data.articles;
  } catch (error) {
    console.error("Error fetching random news: ", error);
    return [];
  }
}

searchButton.addEventListener("click", async () => {
  const query = searchField.value.trim();
  if (query !== "") {
    try {
      const articles = await fetchNewsQuery(query);
      displayBlogs(articles);
    } catch (error) {
      console.error("Error fetching news by query: ", error);
    }
  }
});

async function fetchNewsQuery(query) {
  try {
    const apiUrl = `https://newsapi.org/v2/everything?q=${query}&pageSize=10&apikey=${apiKey}`;
    const response = await fetch(apiUrl);
    const data = await response.json();
    return data.articles;
  } catch (error) {
    console.error("Error fetching news by query: ", error);
    return [];
  }
}

function displayBlogs(articles) {
  blogContainer.innerHTML = "";
  articles.forEach((article) => {
    const blogCard = document.createElement("div");
    blogCard.classList.add("blog-card");

    const img = document.createElement("img");
    img.src = article.urlToImage;
    img.alt = article.title;

    const title = document.createElement("h2");
    const truncatedTitle = article.title.length > 40 ? article.title.slice(0, 40) + "..." : article.title;
    title.textContent = truncatedTitle;

    const description = document.createElement("p");
    const truncatedDes = article.description.length > 120 ? article.description.slice(0, 120) + "..." : article.description;
    description.textContent = truncatedDes;

    blogCard.appendChild(img);
    blogCard.appendChild(title);
    blogCard.appendChild(description);
    blogCard.addEventListener("click", () => {
      window.open(article.url, "_blank");
    });

    blogContainer.appendChild(blogCard);
  });
}

(async () => {
  try {
    const articles = await fetchRandomNews();
    displayBlogs(articles);
  } catch (error) {
    console.error("Error fetching random news: ", error);
  }
})();
