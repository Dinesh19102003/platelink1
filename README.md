PROJECT TITLE

Plate Link-Connecting leftover plates to hungry hands

PROJECT OVERVIEW

Tagline:-One Plate,One Act,One Change
Vision:- A fast human-centered way to move surplus meals to nearby shelters with live tracking and measurable impact.
Mission:- Save Food. Feed People. Build Zero Waste Cities.


Introduction
PlateLink is a platform designed to combat food waste and hunger by creating a seamless, real-time connection between restaurants with surplus food, NGOs in need, and volunteers ready to deliver. It provides full visibility into the donation process via location and quantifies the social and environmental impact.

 

Project SetUp and Installation Guide

Prerequisites

·      Node.js

·      Superbase Account

 

Installation Guide

a)     Create a Project file

 

b)    SetUp SuperBase

·      Create a new project at supabase.com

·      Note your Project URL and API keys

·      Set up database tables using the schema provided below

 

c)     Configure Environment Variables

const CONFIG = {

  SUPABASE_URL: 'YOUR_SUPABASE_URL',

  SUPABASE_ANON_KEY: 'YOUR_SUPABASE_ANON_KEY',

  GOOGLE_MAPS_API_KEY: 'YOUR_GOOGLE_MAPS_API_KEY', // Optional

};

 

d)    Database Schema
-- Users table (extends Superbase auth.users)

CREATE TABLE public.user_profiles (

  id UUID REFERENCES auth.users(id) PRIMARY KEY,

  full_name TEXT,

  phone TEXT,

  role TEXT CHECK (role IN ('restaurant', 'ngo', 'volunteer')),

  created_at TIMESTAMP WITH TIME ZONE DEFAULT TIMEZONE('utc'::text, NOW()),

  updated_at TIMESTAMP WITH TIME ZONE DEFAULT TIMEZONE('utc'::text, NOW())

);

 

-- Donations table

CREATE TABLE public.donations (

  id UUID DEFAULT gen_random_uuid() PRIMARY KEY,

  user_id UUID REFERENCES auth.users(id) NOT NULL,

  food_type TEXT NOT NULL,

  quantity INTEGER NOT NULL CHECK (quantity > 0),

  expiry_hours INTEGER NOT NULL,

  notes TEXT,

  status TEXT DEFAULT 'active' CHECK (status IN ('active', 'matched', 'in-progress', 'completed', 'expired')),

  ngo_id UUID REFERENCES auth.users(id),

  volunteer_id UUID REFERENCES auth.users(id),

  created_at TIMESTAMP WITH TIME ZONE DEFAULT TIMEZONE('utc'::text, NOW()),

  matched_at TIMESTAMP WITH TIME ZONE,

  assigned_at TIMESTAMP WITH TIME ZONE,

  completed_at TIMESTAMP WITH TIME ZONE

);

 

-- User locations table for real-time tracking

CREATE TABLE public.user_locations (

  id UUID DEFAULT gen_random_uuid() PRIMARY KEY,

  user_id UUID REFERENCES auth.users(id) NOT NULL,

  latitude DOUBLE PRECISION NOT NULL,

  longitude DOUBLE PRECISION NOT NULL,

  updated_at TIMESTAMP WITH TIME ZONE DEFAULT TIMEZONE('utc'::text, NOW())

);

 

-- Enable realtime

ALTER PUBLICATION superbase Realtime ADD TABLE donations;

ALTER PUBLICATION superbase_realtime ADD TABLE user_locations;

 

e)     Deploy the application

·      Upload the HTML file to any static hosting service

·      Ensure CORS is properly configured for your Superbase URL.

 

 

 

 

 


Project Architecture

Front End Structure


PlateLink/

├── index.html              # Main application file

├── (imaginary structure)

│   ├── components/         # UI components

│   ├── managers/          # Business logic handlers

│   ├── utils/             # Utility functions

│   └── styles/            # CSS styles

 

Key Architectural Components

1.          Managers Pattern

•            AuthManager: Handles user authentication

•            DatabaseManager: Database operations with retry logic

•            LocationManager: Geolocation services

•            RealtimeManager: Real-time subscriptions

 

2.          State Management

•            Centralized appState object manages application state

•            Real-time updates via Superbase subscriptions

 

3.          Component-Based UI

•            Role-specific dashboards

•            Modular card components

•            Responsive design

 

Code Functionalities


Core Features

1.     Authentication System

•            Email/password signup and login

•            Role-based access (Restaurant, NGO, Volunteer)

•            Guest/demo mode

•            Persistent sessions

 

2.     Restaurant Dashboard

•            Report food surplus with details

•            Track active donations

•            View impact metrics (meals donated, CO₂ saved)

•            Real-time status updates

 

3.     NGO Dashboard

•            Browse available donations

•            Accept donation requests

•            Track incoming deliveries

•            Capacity management

 

4.     Volunteer Dashboard

•            View available delivery jobs

•            Accept delivery assignments

•            Real-time location tracking

•            Delivery completion system

 

5.     Real-Time Features

•            Live donation updates

•            Location tracking

•            Instant notifications

•            Status changes

 

 

Key JavaScript Components

 

1.          Database Manager

•            Retry logic for failed operations

•            Unified interface for all database operations

•            Error handling and fallbacks

 

2.          Authentication Flow

•            Secure session management

•            Role-based routing

•            Protected routes

 

3.          Real-Time Updates

•            WebSocket connections to Superbase

•            Efficient subscription management

•            Automatic reconnection logic

 

4.          Location Services

•            Geolocation API integration

•            Background location updates

•            Distance calculations

 

Implementation Guide

Adding New Features

 

1.          New Database Table

•            Create table in Superbase

•            Add to realtime publications

•            Extend DatabaseManager with CRUD operations

 

2.          New UI Component

•            Create component HTML structure

•            Add corresponding CSS styles

•            Implement JavaScript functionality

 

 

 

 

3.          New Real-Time Event

•            Add new subscription in RealtimeManager

•            Implement event handler

•            Update UI accordingly

 

Customization Options

 

1.          Styling

•            Modify CSS variables in the <style> section

•            Adjust color scheme to match branding

•            Customize card styles and animations

 

2.          Functionality

•            Add new donation types

•            Implement payment integration for volunteers

•            Add advanced reporting features

 

Performance Considerations

 

1.          Optimized Real-Time Updates

•            Efficient subscription management

•            Debounced location updates

•            Selective data fetching

 

2.          Caching Strategy

•            Local state management

•            Optimistic UI updates

•            Efficient re-rendering

 

3.          Bundle Size

•            Minimal dependencies

•            Efficient icon usage

•            Compressed assets

 

Security Measures

 

1.          Row Level Security (RLS)

•            Enable RLS on all tables

•            Implement proper policies in Superbase

 

2.          Input Validation

•            Client-side validation

•            Server-side validation via database constraints

 

3.          Authentication

•            Secure session management

•            Role-based access control

 

Browser Compatibility

•            Chrome (recommended)

•            Firefox

•            Safari

•            Edge

 

 

Troubleshooting

Common Issues

1.          Superbase Connection Errors

•            Check API URL and keys

•            Verify database tables exist

•            Check CORS settings

2.          Location Services Not Working

•            Ensure HTTPS for geolocation

•            Check browser permissions

3.          Real-Time Updates Not Working

•            Verify Realtime is enabled in Superbase

•            Check subscription code
