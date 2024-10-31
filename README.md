# HashTable
import React, { useEffect, useState } from "react";
import Input from "../../Components/Input/Input";
import classes from "./Home.module.css";

const questions = ["q1", "q2", "q3", "q4"];
const subQuestions = ["sq1", "sq2", "sq3", "sq4"];
const answers = ["a1", "a2", "a3", "a4"];

function Home() {
  const [mainState, setMainState] = useState(new Map());

  const handleFormMapping = () => {
    const map = new Map();
    for (const quesVal of questions) {
      map.set(`${quesVal}`, "");
      for (const subQueVal of subQuestions) {
        map.set(`${quesVal}-${subQueVal}`, "");
      }
      for (const ansVal of answers) {
        map.set(`${quesVal}-${ansVal}`, "");
      }
    }
    setMainState(map);
  };

  useEffect(() => {
    handleFormMapping();
  }, []);

  const handleChange = (key, value) => {
    setMainState((prev) => {
      const newMap = new Map(prev);
      newMap.set(key, value);
      return newMap;
    });
  };

  const convertMapKeysToArrayFormat = (map) => {
    const convertedObject = {};
    map.forEach((value, key) => {
      const keyArray = key.split("-");
      convertedObject[keyArray] = value;
    });
    return convertedObject;
  };

  const handleSubmit = () => {
    const convertedData = convertMapKeysToArrayFormat(mainState);
    console.log("Data to send to backend:", convertedData);
  };

  return (
    <div className={classes.main}>
      <h1>Home</h1>
      {questions.map((ele, index) => (
        <Input
          key={index}
          setter={(e) => handleChange(ele, e)}
          value={mainState.get(ele) || ""}
          label={"Question"}
          type={"text"}
        />
      ))}
      <button onClick={handleSubmit}>Submit</button>
    </div>
  );
}

export default Home;
