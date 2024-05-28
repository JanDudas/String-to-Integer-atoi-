# String-to-Integer-atoi-
/**
 * @param {string} s
 * @return {number}
 */
var myAtoi = function (s) {
    let
        result = 0, stringLength = s.length;
    if (s == null || stringLength == 0) {
        return result;
    } else {
        if (0 > s.length || s.length > 200) {
            return result;
        }
        s = s.trim();
        s = s + " ";

        let
            isNegativeNumber = false, numberStarted = false, stringNumber = "",
            countNumbers = 11;
        for (var int = 0; int < stringLength; int++) {
            let
                letter = s.charAt(int), nextLetter = s.charAt(int + 1),
                numberLetterASCII = letter.charCodeAt(),
                nextNumberLetterASCII = nextLetter.charCodeAt();

            switch (numberLetterASCII) {
                case 45: // '-'
                    if (nextNumberLetterASCII < 48 || nextNumberLetterASCII > 58) {
                        return 0;
                    }
                    isNegativeNumber = true;
                    break;

                case 43: // '+'
                    if (nextNumberLetterASCII < 48 || nextNumberLetterASCII > 58) {
                        return 0;
                    }
                    break;

                case 48: // '0'
                    switch (numberStarted) {
                        case false:
                            if (nextNumberLetterASCII < 48
                                || nextNumberLetterASCII > 57) {
                                numberStarted = true;
                                result = parseInt(Number(letter));
                                int = stringLength - 1;
                            } else {
                                if ((nextNumberLetterASCII >= 48 && nextNumberLetterASCII <= 57)) {
                                    continue;
                                }
                            }
                            break;

                        case true:
                            if (nextNumberLetterASCII < 48
                                || nextNumberLetterASCII > 57) {
                                stringNumber = stringNumber + letter;
                                if (stringNumber.length >= countNumbers) {
                                    result = BigInt(stringNumber);
                                } else {
                                    result = parseInt(Number(stringNumber));
                                }
                                int = stringLength - 1;
                            } else {
                                if (nextNumberLetterASCII >= 48
                                    && nextNumberLetterASCII <= 57) {
                                    stringNumber = stringNumber + letter;
                                }
                            }
                            break;

                        default:
                            break;
                    }
                    break;

                default:
                    if ((numberLetterASCII >= 49 && numberLetterASCII <= 57)
                        && (nextNumberLetterASCII < 48 || nextNumberLetterASCII > 57)) {
                        numberStarted = true;
                        stringNumber = stringNumber + letter;
                        if (stringNumber.length >= countNumbers) {
                            result = BigInt(stringNumber);
                        } else {
                            result = parseInt(Number(stringNumber));
                        }
                        int = stringLength - 1;
                    } else {
                        if ((numberLetterASCII >= 49 && numberLetterASCII <= 57)
                            && (nextNumberLetterASCII >= 48 && nextNumberLetterASCII <= 57)) {
                            numberStarted = true;
                            stringNumber = stringNumber + letter;
                        } else {
                            if ((numberLetterASCII < 49 || numberLetterASCII > 57)) {
                                result = 0;
                                int = stringLength - 1;
                            }
                        }
                    }
                    break;
            }
        }

        let
            lowest = (Math.pow(-2, 31)), hghest = (Math.pow(2, 31) - 1);
        switch (Number.isSafeInteger(result)) {
            case false:
                if (isNegativeNumber) {
                    return lowest;
                } else {
                    return hghest;
                }
                break;

            case true:
                if (isNegativeNumber) {
                    result = result * (-1);
                }
                if (result < lowest) {
                    result = lowest;
                } else {
                    if (result > hghest) {
                        result = hghest;
                    }
                }
                break;
            default:
                break;
        }
    }
    return result;
};
