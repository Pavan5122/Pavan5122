npx create-react-app weather-app
cd weather-app
npm install axios
import axios from 'axios';
const API_KEY = 'your_openweathermap_api_key';
export const fetchWeatherData = async (city) => {
  try {
    const response = await axios.get(`https://api.openweathermap.org/data/2.5/weather?q=${city}&appid=${API_KEY}&units=metric`);
    return response.data;
  } catch (error) {
    throw error;
  }
};
// Form.js

import React, { useState } from 'react';

const Form = ({ onSearch }) => {
  const [city, setCity] = useState('');

  const handleChange = (e) => {
    setCity(e.target.value);
  };

  const handleSubmit = (e) => {
    e.preventDefault();
    onSearch(city);
    setCity('');
  };

  return (
    <form onSubmit={handleSubmit}>
      <input
        type="text"
        placeholder="Enter city name"
        value={city}
        onChange={handleChange}
      />
      <button type="submit">Search</button>
    </form>
  );
};

export default Form;
// WeatherCard.js

import React from 'react';

const WeatherCard = ({ city, temperature, description }) => {
  return (
    <div className="weather-card">
      <h2>{city}</h2>
      <p>{temperature}°C</p>
      <p>{description}</p>
    </div>
  );
};

export default WeatherCard;
// WeatherDashboard.js

import React, { useState } from 'react';
import { fetchWeatherData } from './weatherService';
import Form from './Form';
import WeatherCard from './WeatherCard';

const WeatherDashboard = () => {
  const [weatherData, setWeatherData] = useState(null);
  const [error, setError] = useState('');

  const handleSearch = async (city) => {
    try {
      const data = await fetchWeatherData(city);
      setWeatherData(data);
      setError('');
    } catch (error) {
      setError('City not found. Please try again.');
    }
  };

  return (
    <div className="weather-dashboard">
      <Form onSearch={handleSearch} />
      {error && <p className="error">{error}</p>}
      {weatherData && (
        <WeatherCard
          city={weatherData.name}
          temperature={weatherData.main.temp}
          description={weatherData.weather[0].description}
        />
      )}
    </div>
  );
};

export default WeatherDashboard;

