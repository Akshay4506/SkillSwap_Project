
``` jsx
import React, { useState } from "react";

import { useNavigate } from "react-router-dom";

import "../styles/SignupForm.css";

import SignupImage from "../assets/signupimage.webp";

import axios from "axios";

function Signup() {

    const navigate = useNavigate();

    const [formData, setFormData] = useState({

        fullName: "",

        email: "",

        password: "",

        confirmPassword: "",

        year: "",

        branch: "",

        profileImage: null,

        skills:[],

    });

  

    const [errors, setErrors] = useState({});

    const [skillInput, setSkillInput]=useState("");

    const handleChange = (e) => {

        const { name, value } = e.target;

        setFormData({ ...formData, [name]: value });

    };

    const handleSkillChange = (e) => {

        setSkillInput(e.target.value);

    };

    const handleFileChange = (e) => {

        const file = e.target.files[0];

        if (file) {

            setFormData({ ...formData, profileImage: URL.createObjectURL(file) }); // Preview the image

        }

    };

    const addSkill = (e) => {

        if (e.key === "Enter" && skillInput) {

            e.preventDefault();

            if (!formData.skills.includes(skillInput)) {

                setFormData({

                    ...formData,

                    skills: [...formData.skills, skillInput],

                });

                setSkillInput("");

            }

        }

    };

  

    const removeSkill = (skill) => {

        setFormData({

            ...formData,

            skills: formData.skills.filter((s) => s !== skill),

        });

    };

    const validateForm = () => {

        const newErrors = {};

        if (!formData.fullName.trim()) newErrors.fullName = "Full Name is required";

        if (!formData.email.trim()) newErrors.email = "Email is required";

        else if (!/\S+@\S+\.\S+/.test(formData.email)) newErrors.email = "Invalid email format";

        if (!formData.password) newErrors.password = "Password is required";

        else if (formData.password.length < 6) newErrors.password = "Password must be at least 6 characters";

        if (formData.password !== formData.confirmPassword) newErrors.confirmPassword = "Passwords do not match";

        if (!formData.year) newErrors.year = "Please select your year";

        if (!formData.branch) newErrors.branch = "Please select your branch";

        if (!formData.profileImage) newErrors.profileImage = "Profile image is required";

        if (formData.skills.length===0)newErrors.skills="Add at least one skill";

  

        setErrors(newErrors);

        return Object.keys(newErrors).length === 0;

    };

  

    const handleSubmit = async (e) => {

        e.preventDefault();

        if (!validateForm()) return;

        const formDataToSend = new FormData(); // Using FormData to handle file uploads

        formDataToSend.append("fullName", formData.fullName);

        formDataToSend.append("email", formData.email);

        formDataToSend.append("password", formData.password);

        formDataToSend.append("confirmPassword", formData.confirmPassword);

        formDataToSend.append("year", formData.year);

        formDataToSend.append("branch", formData.branch);

        formDataToSend.append("profileImage", formData.profileImage); // If you plan to send the image as a file, you might need to handle it properly in the backend

        formData.skills.forEach(skill => formDataToSend.append("skills[]", skill)); // Append skills as an array

        try {

            // Make the POST request to the backend

            const response = await axios.post("http://localhost:5000/api/register", formDataToSend, {

                headers: {

                    'Content-Type': 'multipart/form-data' // This is important when sending files

                }

            });

            console.log("Account created successfully:", response.data);

            alert("Account created successfully!");

            navigate("/login");

        } catch (error) {

            console.error("Error creating account:", error);

            alert("Error creating account. Please try again.");

        }

    };

  

    return (

        <div className="signup-container">

            <div className="form-section">

                <h1>Create an Account</h1>

  

                <form onSubmit={handleSubmit}>

                    <div className="form-group">

                        <label htmlFor="fullName">Full Name</label>

                        <input

                            type="text"

                            id="fullName"

                            name="fullName"

                            placeholder="Enter your full name"

                            value={formData.fullName}

                            onChange={handleChange}

                        />

                        {errors.fullName && <span className="error-text">{errors.fullName}</span>}

                    </div>

  

                    <div className="form-group">

                        <label htmlFor="email">Email</label>

                        <input

                            type="email"

                            id="email"

                            name="email"

                            placeholder="Enter your email"

                            value={formData.email}

                            onChange={handleChange}

                        />

                        {errors.email && <span className="error-text">{errors.email}</span>}

                    </div>

  

                    <div className="form-group">

                        <label htmlFor="password">Password</label>

                        <input

                            type="password"

                            id="password"

                            name="password"

                            placeholder="Enter your password"

                            value={formData.password}

                            onChange={handleChange}

                        />

                        {errors.password && <span className="error-text">{errors.password}</span>}

                    </div>

  

                    <div className="form-group">

                        <label htmlFor="confirmPassword">Confirm Password</label>

                        <input

                            type="password"

                            id="confirmPassword"

                            name="confirmPassword"

                            placeholder="Re-enter your password"

                            value={formData.confirmPassword}

                            onChange={handleChange}

                        />

                        {errors.confirmPassword && <span className="error-text">{errors.confirmPassword}</span>}

                    </div>

  

                    <div className="form-group">

                        <label htmlFor="year">Year</label>

                        <select

                            id="year"

                            name="year"

                            value={formData.year}

                            onChange={handleChange}

                        >

                            <option value="">Select Year</option>

                            <option value="1">1st Year</option>

                            <option value="2">2nd Year</option>

                            <option value="3">3rd Year</option>

                            <option value="4">4th Year</option>

                        </select>

                        {errors.year && <span className="error-text">{errors.year}</span>}

                    </div>

  

                    <div className="form-group">

                        <label htmlFor="branch">Branch</label>

                        <select

                            id="branch"

                            name="branch"

                            value={formData.branch}

                            onChange={handleChange}

                        >

                            <option value="">Select Branch</option>

                            <option value="CSE">Computer Science Engineering</option>

                            <option value="ISE">Information Science Engineering</option>

                            <option value="ECE">Electronics and Communication Engineering</option>

                            <option value="ME">Mechanical Engineering</option>

                            <option value="CV">Civil Engineering</option>

                            <option value="CH">Chemical Engineering</option>

                            <option value="AE">Aeronautical Engineering</option>

                            <option value="IEM">Industrial Management</option>

                        </select>

                        {errors.branch && <span className="error-text">{errors.branch}</span>}

                    </div>

  

                    <div className="form-group">

                        <label htmlFor="profileImage">Profile Image</label>

                        <input

                            type="file"

                            id="profileImage"

                            name="profileImage"

                            accept="image/*"

                            onChange={handleFileChange}

                        />

                        {errors.profileImage && <span className="error-text">{errors.profileImage}</span>}

                        {formData.profileImage && <img src={formData.profileImage} alt="Profile Preview" className="image-preview" />}

                    </div>

                    <div className="form-group">

                    </div>

  

                    <button type="submit" className="primary-btn">

                        Create Account

                    </button>

                    <button

                        type="button"

                        className="secondary-btn"

                        onClick={() => navigate("/login")}

                    >

                        Sign In

                    </button>

                </form>

            </div>

            <div className="image-section">

                <img src={SignupImage} alt="Illustration" />

            </div>

        </div>

    );

}

  

export default Signup;
```


